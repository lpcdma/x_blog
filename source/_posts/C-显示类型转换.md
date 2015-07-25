title: C++显示类型转换
date: 2015-07-25 15:49:41
tags:
categories:
  - 学习笔记
---

标准C++中有四种类型转换符：static_cast,dynamic_cast,reinterpret_cast,const_cast.
##static_cast
- 1.void指针->其他类型的指针
- 2.改变通常的标准转换
- 3.避免出现可能多种转换的歧义

##dynamic_cast
- 1.无法使用virtual函数的时候
- 2.操作对象必须是类的指针，引用或者void指针

##reinterpret_cast
- 1.不提倡使用
- 2.操作对象是指针，引用，算数类型，函数指针类型，成员指针

##const_cast

- 1.主要用于修改const或volatile属性

#static_cast&reinterpret_cast举例
```cpp
#include <stdlib.h>
#include <stdio.h>
#include <iostream>

struct data {
    int nr;
    char const *value;
} dat[] = {
    {1, "Foo"}, {2, "Bar"}, {3, "Hello"}, {4, "World"}
};

int data_cmp(void const *lhs, void const *rhs) 
{
    struct data const *const l = reinterpret_cast<const struct data *>(lhs);
    struct data const *const r = reinterpret_cast<const struct data *>(rhs);
    return (l->nr > r->nr) - (l->nr < r->nr);
}

int main(void) 
{
    struct data key = { .nr = 3 };
    struct data const *res = reinterpret_cast<struct data *>(bsearch(&key, dat, sizeof(dat)/sizeof(dat[0]), 
                                    sizeof(dat[0]), data_cmp));
    if (!res) {
        printf("No %d not found\n", key.nr);
    } else {
        printf("No %d: %s\n", res->nr, res->value);
    }
}
```
#dynamic_cast举例
```cpp
#include <iostream>
 
struct V {
    virtual void f() {};  // must be polymorphic to use runtime-checked dynamic_cast
};
struct A : virtual V {};
struct B : virtual V {
  B(V* v, A* a) {
    // casts during construction (see the call in the constructor of D below)
    dynamic_cast<B*>(v); // well-defined: v of type V*, V base of B, results in B*
    dynamic_cast<B*>(a); // undefined behavior: a has type A*, A not a base of B
  }
};
struct D : A, B {
    D() : B((A*)this, this) { }
};
 
struct Base {
    virtual ~Base() {}
};
 
struct Derived: Base {
    virtual void name() {}
};
 
int main()
{
    D d; // the most derived object
    A& a = d; // upcast, dynamic_cast may be used, but unnecessary
    D& new_d = dynamic_cast<D&>(a); // downcast
    B& new_b = dynamic_cast<B&>(a); // sidecast
 
 
    Base* b1 = new Base;
    if(Derived* d = dynamic_cast<Derived*>(b1))
    {
        std::cout << "downcast from b1 to d successful\n";
        d->name(); // safe to call
    }
 
    Base* b2 = new Derived;
    if(Derived* d = dynamic_cast<Derived*>(b2))
    {
        std::cout << "downcast from b2 to d successful\n";
        d->name(); // safe to call
    }
 
    delete b1;
    delete b2;
}
```