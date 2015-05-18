title: "beef在bt5下不能运行解决办法"
date: 2012-06-13 13:22:53
tags:
id: 14
comment: false
---

 这个问题是与环境变量有关。
 这是一个偶然，晚上在使用wpscan时升级后发现不能用了，提示安装什么东东，
 安装了也不行，
 然后看到提示后面有个or
 [code lang="c"]gem install –user-install[/code]
 用这个命令安装好就可以了，灵机一动那么beef是否也可以呢？
 结果肯定是令人欣慰的！！！！
 在msf中使用可能遇到–hpricot这类问题。
 安装
 [code lang="c"]gem isntall –user-install hpricot or gem install hpricot[/code]
 都行不通后也是有办法的，
 原因是在bt5中，msf使用的ruby环境变量是在：
 [code lang="c"]root@bt:/opt/metasploit/ruby#[/code]
 所以也就找不到hpricot
 使用
[code lang="c"]
 root@bt:/opt/metasploit/ruby# cd /
 root@bt:/# find -name hpricot
 ./var/lib/gems/1.9.2/doc/hpricot-0.8.6/rdoc/lib/hpricot
 ./var/lib/gems/1.9.2/gems/hpricot-0.8.6/lib/hpricot
 ./var/lib/gems/1.8/doc/hpricot-0.8.4/rdoc/files/lib/hpricot
 ./var/lib/gems/1.8/gems/hpricot-0.8.4/lib/hpricot
 ./root/.gem/ruby/1.9.2/doc/hpricot-0.8.6/rdoc/lib/hpricot
 ./root/.gem/ruby/1.9.2/gems/hpricot-0.8.6/lib/hpricot
 root@bt:/#cp /root/.gem/ruby/1.9.2/gems/hpricot-0.8.6/ /opt/metasploit/ruby/lib/ruby/gems/1.9.1/gems/
[/code]
 这样问题就能解决。。。
[code lang="c"]
root@bt:/pentest/web/beef# ./beef
/pentest/web/beef/core/loader.rb:18:in `require’: no such file to load — bundler/setup (LoadError)
from /pentest/web/beef/core/loader.rb:18:in `&lt;top (required)&gt;’
from ./beef:42:in `require’         from ./beef:42:in `&lt;main&gt;’
root@bt:/pentest/web/beef# gem install –user-install bundler
WARNING:  You don’t have /root/.gem/ruby/1.9.2/bin in your PATH,
          gem executables will not run.
Successfully installed bundler-1.1.4
1 gem installed
Installing ri documentation for bundler-1.1.4…
Installing RDoc documentation for bundler-1.1.4… 
root@bt:/pentest/web/beef# ls
beef  config.yaml  core  extensions  Gemfile  Gemfile.lock  install  modules  Rakefile  README  test  VERSION
root@bt:/pentest/web/beef# ./beef 
[21:28:09][*] Browser Exploitation Framework (BeEF)
[21:28:09]    |   Version 0.4.3.2-alpha
[21:28:09]    |   Website http://beefproject.com
[21:28:09]    |   Run ‘beef -h’ for basic help.
[21:28:09]    |_  Run ‘git pull’ to update to the latest revision.
[21:28:09][*] BeEF is loading. Wait a few seconds…
[21:28:10][*] 9 extensions loaded:
[21:28:10]    |   Demos
[21:28:10]    |   Requester
[21:28:10]    |   Autoloader
[21:28:10]    |   XSSRays
[21:28:10]    |   Console
[21:28:10]    |   Proxy
[21:28:10]    |   Initialization
[21:28:10]    |   Events
[21:28:10]    |_  Admin UI
[21:28:10][*] 77 modules enabled.
[21:28:10][*] 2 network interfaces were detected.
[21:28:10][+] running on network interface: 127.0.0.1
[21:28:10]    |   Hook URL: http://127.0.0.1:3000/hook.js
[21:28:10]    |_  UI URL:   http://127.0.0.1:3000/ui/panel
[21:28:10][+] running on network interface: 180.8*.*.*
[21:28:10]    |   Hook URL: http://180.8*.*.*:3000/hook.js
[21:28:10]    |_  UI URL:   http://180.8*.*.*:3000/ui/panel
[21:28:10][+] HTTP Proxy: http://127.0.0.1:6789
[21:28:10][*] BeEF server started (press control+c to stop)
[/code]