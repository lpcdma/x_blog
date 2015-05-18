title: "LINUX ELF HOOK DEMO源码"
date: 2013-10-27 22:04:50
tags:
id: 330
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">#define _GNU_SOURCE
#define __USE_GNU

#include &lt;sys/stat.h&gt;
#include &lt;fcntl.h&gt;
#include &lt;sys/mman.h&gt;

#include &lt;signal.h&gt;
#include &lt;dlfcn.h&gt;
#include &lt;errno.h&gt;

#include &lt;elf.h&gt;
#include &lt;link.h&gt;
#include &lt;stdlib.h&gt;
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;

extern int test_so_call(int a, int b);

#define ELFW(name, t) _ELFW (name, __WORDSIZE, t)
#define _ELFW(name, bits, t) __ELFW (name, bits, t)
#define __ELFW(name, bits, t) name##bits##t

#define SYMID_2_NAME(e_info, i) \
        (e_info)-&gt;e_strtab + (e_info)-&gt;e_syms[i].st_name

#define PROT_ADDR(addr, t) \
        mprotect((void*)((size_t)(addr) &amp; (((size_t)-1) ^ 0xfff)), 0x1000, t)

typedef struct _elf_info {
        ElfW(Ehdr) *e_ehdr;
        ElfW(Shdr) *e_shdr;
        ElfW(Phdr) *e_phdr;
        ElfW(Shdr) *e_shdr_dyn;
        ElfW(Sym)  *e_syms;

        ElfW(Rela) *e_rela;
        ElfW(Rel) *e_rel;
        size_t e_relsz;

        /* jmp plt import */
        char* e_jmprel;
        size_t e_pltrel;
        size_t e_pltrelsz;

        uint32_t *e_hashtab;
        char* e_strtab;
        void** e_got;
        size_t e_dyn_size;

        size_t e_shnum;
        int e_class;            /* ELFCLASS32/ELFCLASS64 */

} elf_info, *pelf_info;

typedef struct _elf_hook_item {
        void* modbase;
        const char* lib_name;
        const char* fun_name;
        void* hook_func;
        void* org_func;
        void** porg_func;
} elf_hook_item, *pelf_hook_item;

elf_hook_item hook_test;
elf_hook_item hook_test2;

int hook_so_call(int a, int b)
{
        printf("!!!!!eat hook test_so_call\n");

        return ((int(*)(int,int))hook_test.org_func)(a, b) + 1;
}

#include &lt;stdarg.h&gt;

int hook_printf(__const char *__restrict __format, va_list ap)
{
        int ret = 0;
        printf("!!!!!iat hook printf\n");

        //printf(__format, ap);
        ret = ((int(*)(const char *,...))hook_test2.org_func)(__format, ap);

        printf("printf:GOT %p -&gt; %p\n", hook_test2.porg_func, *hook_test2.porg_func);
        if (*hook_test2.porg_func != hook_printf)
                *hook_test2.porg_func = hook_printf;

        return ret;
}

unsigned long elf_Hash(const char *name)
{
        unsigned long h = 0, g;

        while (*name) {
                h = (h &lt;&lt; 4) + *name++;
                if ((g = h &amp; 0xf0000000))
                        h ^= g &gt;&gt; 24;
                h &amp;= ~g;
        }
        return h;
}

static ElfW(Sym)* get_func_sym(pelf_info e_info, const char* fun_name)
{
        unsigned long hash;
        uint32_t *buf = e_info-&gt;e_hashtab;
        uint32_t nbucket, nchain;
        uint32_t *bucket, *chain;
        ElfW(Sym)* e_sym = e_info-&gt;e_syms;
        uint32_t symindx;

        hash = elf_Hash(fun_name);

        nbucket = buf[0];
        nchain = buf[1];

        bucket = buf + 2;
        chain = bucket + nbucket;

        for (symindx=bucket[hash % nbucket];
                        symindx != STN_UNDEF; symindx=chain[symindx]) {
                //printf("%s\n", e_info-&gt;e_strtab + e_sym[symindx].st_name);
                if (strcmp(e_info-&gt;e_strtab + e_sym[symindx].st_name, fun_name) == 0)
                        return e_sym + symindx;
        }

        return NULL;
}

static void __parse_dyn_info(void* dyntab, pelf_info e_info)
{
        ElfW(Dyn)* e_dyn;

        for (e_dyn = (ElfW(Dyn)*)dyntab; e_dyn-&gt;d_tag != DT_NULL; e_dyn++) {
                //printf("%d\n", e_dyn-&gt;d_tag);
                switch (e_dyn-&gt;d_tag) {
                case DT_PLTGOT:
                        e_info-&gt;e_got =  (void**)e_dyn-&gt;d_un.d_ptr;
                        break;
                case DT_STRTAB:
                        e_info-&gt;e_strtab = (char*)e_dyn-&gt;d_un.d_ptr;
                        break;
                case DT_HASH:
                        e_info-&gt;e_hashtab = (uint32_t *)e_dyn-&gt;d_un.d_ptr;
                        break;
                case DT_SYMTAB:
                        e_info-&gt;e_syms = (ElfW(Sym)*)e_dyn-&gt;d_un.d_ptr;
                        break;
                case DT_REL:
                        e_info-&gt;e_rel = (ElfW(Rel)*)e_dyn-&gt;d_un.d_ptr;
                        break;
                case DT_RELA:
                        e_info-&gt;e_rela = (ElfW(Rela)*)e_dyn-&gt;d_un.d_ptr;
                        break;
                case DT_JMPREL:
                        e_info-&gt;e_jmprel = (char*)e_dyn-&gt;d_un.d_ptr;
                        break;
                case DT_PLTREL:
                        e_info-&gt;e_pltrel = e_dyn-&gt;d_un.d_val;
                        break;
                case DT_PLTRELSZ:
                        e_info-&gt;e_pltrelsz = e_dyn-&gt;d_un.d_val;
                        break;
                case DT_RELSZ:
                case DT_RELASZ:
                        e_info-&gt;e_relsz = e_dyn-&gt;d_un.d_val;
                        break;
                default:
                        continue;
                }
        }
}

static int __do_eat_hook(char* modbase, pelf_hook_item hook_item)
{
        elf_info e_info = {0};
        ElfW(Sym)* e_sym;
        int i;

        e_info.e_class = modbase[EI_CLASS];
        e_info.e_ehdr = (ElfW(Ehdr)*)modbase;
        e_info.e_shdr = (ElfW(Shdr)*)(modbase + e_info.e_ehdr-&gt;e_shoff);
        e_info.e_phdr = (ElfW(Phdr)*)(modbase + e_info.e_ehdr-&gt;e_phoff);

        /* get dynamin info */
        for (i=0; i&lt;e_info.e_ehdr-&gt;e_phnum; i++) {
                if (e_info.e_phdr[i].p_type == PT_DYNAMIC) {
                        e_info.e_shdr_dyn = (ElfW(Shdr) *)(modbase + e_info.e_phdr[i].p_vaddr);
                        e_info.e_dyn_size = e_info.e_phdr[i].p_memsz;
                        break;
                }
        }

        if (!e_info.e_shdr_dyn)
                return -1;

        __parse_dyn_info(e_info.e_shdr_dyn, &amp;e_info);

        printf("base %p %p hash %p\n", modbase, e_info.e_syms, e_info.e_hashtab);

        e_sym = get_func_sym(&amp;e_info, hook_item-&gt;fun_name);
        if (e_sym == NULL) {
                printf("get_func_sym:%s failed\n", hook_item-&gt;fun_name);
                return -1;
        }

        if (PROT_ADDR(e_sym, PROT_READ | PROT_WRITE | PROT_EXEC) == -1) {
                printf("mprotect error! %d\n", errno);
                return -1;
        }

        hook_item-&gt;modbase = modbase;
        hook_item-&gt;org_func = e_sym-&gt;st_value + modbase;
        hook_item-&gt;porg_func = (void**)&amp;e_sym-&gt;st_value;
        e_sym-&gt;st_value = (char*)hook_item-&gt;hook_func - modbase;

        printf("eat hook %s %p -&gt; %p\n",
               hook_item-&gt;fun_name,
               hook_item-&gt;porg_func,
               hook_item-&gt;hook_func
               );

        return 0;
}

static int enum_mod_base(struct dl_phdr_info *info, size_t size, void *data)
{
        pelf_hook_item hook_item = (pelf_hook_item)data;
        int i;

        printf("name=%s (%d segments) address=%p\n",
               info-&gt;dlpi_name, info-&gt;dlpi_phnum, (void*)info-&gt;dlpi_addr);

        if (strstr(info-&gt;dlpi_name, hook_item-&gt;lib_name) == NULL)
                return 0;

        for (i = 0; i &lt; info-&gt;dlpi_phnum; i++) {
                if (info-&gt;dlpi_phdr[i].p_type == PT_LOAD) {
                        char* lib_base = (char *)(info-&gt;dlpi_addr +
                                                  info-&gt;dlpi_phdr[i].p_vaddr);
                        __do_eat_hook(lib_base, hook_item);
                        break;
                }
        }

        return 0;
}

static int eat_hook(pelf_hook_item hook_item)
{
        dl_iterate_phdr(enum_mod_base, hook_item);
        return 0;
}

static int iat_hook(pelf_hook_item hook_item)
{
        struct link_map *link_map;
        char* rel_bf;
        int symid;
        void* symaddr;
        elf_info e_info = {0};

        link_map = (struct link_map *)dlopen(hook_item-&gt;lib_name, RTLD_LAZY);
        if (!link_map)
                return -1;

        hook_item-&gt;modbase = (void*)link_map-&gt;l_addr;
        printf("l_ld-&gt;%p\n", link_map-&gt;l_ld);
        __parse_dyn_info(link_map-&gt;l_ld, &amp;e_info);
        dlclose(link_map);

        if (!e_info.e_jmprel)
                return -1;

        for (rel_bf = e_info.e_jmprel; rel_bf &lt; e_info.e_jmprel + e_info.e_pltrelsz;) {
                if (DT_RELA == e_info.e_pltrel) {
                        symid = ELFW(ELF,_R_SYM)(((ElfW(Rela)*)rel_bf)-&gt;r_info);
                        symaddr = (void*)((ElfW(Rela)*)rel_bf)-&gt;r_offset;
                } else {
                        symid = ELFW(ELF,_R_SYM)(((ElfW(Rel)*)rel_bf)-&gt;r_info);
                        symaddr = (void*)((ElfW(Rel)*)rel_bf)-&gt;r_offset;
                }

                if (!strcmp(SYMID_2_NAME(&amp;e_info, symid), hook_item-&gt;fun_name)) {
                        if (PROT_ADDR(symaddr, PROT_READ | PROT_WRITE | PROT_EXEC) == -1) {
                                printf("mprotect error! %d\n", errno);
                                return 0;
                        }

                        hook_item-&gt;porg_func = (void**)symaddr;
                        hook_item-&gt;org_func = *(void**)symaddr;

                        *(void**)symaddr = hook_item-&gt;hook_func;

                        printf("iat hook %s %p -&gt; %p\n",
                               hook_item-&gt;fun_name,
                               hook_item-&gt;porg_func,
                               hook_item-&gt;hook_func
                               );
                }

                rel_bf += (DT_RELA == e_info.e_pltrel) ? sizeof(ElfW(Rela)):sizeof(ElfW(Rel));
        }

        return -1;
}

__attribute ((constructor)) void so_lib_init(void)
{
        printf("hook so init\n");

        hook_test.lib_name = "so_test.so";
        hook_test.fun_name = "test_so_call";
        hook_test.hook_func = hook_so_call;

        eat_hook(&amp;hook_test);

        hook_test2.lib_name = NULL;
        hook_test2.fun_name = "printf";
        hook_test2.hook_func = hook_printf;

        iat_hook(&amp;hook_test2);
}

__attribute ((destructor)) void so_lib_free(void)
{
        printf("so_lib_freed\n");
}
//http://bbs.pediy.com/showthread.php?t=178320</pre>