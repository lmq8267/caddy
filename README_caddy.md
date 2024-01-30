Caddy 是一个用 Go 编写的可扩展服务器平台。

从本质上讲，Caddy 仅管理配置。模块已插入
在编译时静态地提供有用的功能。球童的
标准发行版包括服务 HTTP、TLS 的通用模块，
和 PKI 应用程序，包括证书的自动化。

要运行 Caddy，请使用：

        - “caddy run”在前台运行 Caddy（推荐）。
        - “caddy start”在后台启动 Caddy；只做这个
          如果您要保持终端窗口打开直到运行
          'caddy stop' 关闭服务器。

当 Caddy 启动时，它会打开一个本地绑定的管理套接字
可以通过 Restful HTTP API 将配置发布到其中（请参阅
https://caddyserver.com/docs/api）。

Caddy 的原生配置格式是 JSON。但是，配置适配器
可用于在 Caddy 接收时将其他配置格式转换为 JSON
它的配置。 Caddyfile 是一个内置的配置适配器
由于其简单性，手写配置很受欢迎
语法（请参阅 https://caddyserver.com/docs/caddyfile）。许多第三方
适配器可用（请参阅 https://caddyserver.com/docs/config-adapters）。
使用“caddy Adapt”查看配置如何转换为 JSON。

为了方便起见，CLI 可以充当 HTTP 客户端来为 Caddy 提供
为您提供初始配置。如果名为 Caddyfile 的文件位于
当前工作目录，它会自动执行此操作。否则，
您可以使用 --config 标志来指定配置文件的路径。

一些特殊用途的子命令构建并加载配置文件
直接从命令行输入为您服务；例如：

        - caddy file-server
        - caddy reverse-proxy
        - caddy respond

这些命令禁用管理端点，因为它们
配置仅在命令行上指定。

一般来说，运行 Caddy 最常见的方式很简单：

        $ caddy run

或者，使用配置文件：

        $ caddy run --config caddy.json

如果在终端中交互运行，那么后台运行可能更方便：
        $ caddy start
        ...
        $ caddy stop

这允许您在 Caddy 保持运行的同时运行其他命令。
请务必在关闭终端之前停止 Caddy！

根据系统的不同，Caddy 可能需要权限才能绑定到低
端口。在 Linux 上执行此操作的一种方法是使用 setcap：

        $ sudo setcap cap_net_bind_service=+ep $(which caddy)

请记住在替换二进制文件后再次运行该命令。

请参阅 Caddy 网站了解教程、配置结构、
语法和模块文档：https://caddyserver.com/docs/

自定义 Caddy 版本可在 Caddy 下载页面获取：
https://caddyserver.com/download

xcaddy 命令可用于从源代码构建 Caddy，或者
无需额外插件：https://github.com/caddyserver/xcaddy

如果可能，应使用官方支持的安装 Caddy
软件包安装程序：https://caddyserver.com/docs/install

还提供了在生产环境中运行 Caddy 的说明：
https://caddyserver.com/docs/running

用法：
  caddy [命令]

例子：
```shell
  $ caddy run
  $ caddy run --config caddy.json
  $ caddy reload --config caddy.json
  $ caddy stop
```

可用命令：
```shell
  adapt               将Caddyfile转为JSON原生配置
  add-package   添加 Caddy 包（实验）
  build-info        打印有关此二进制文件的构建信息
  completion      生成完成脚本
  environ            打印caddy和系统的环境变量
  file-server        启动生产就绪的文件服务器
  fmt                  修饰配置文件Caddyfile，使其更整洁整齐
  hash-password  将明文密码进行哈希处理生成 base64 密码
  help                 caddy命令解释
  list-modules    列出 Caddy当前已安装的所有模块
  manpage         生成 Caddy 命令的手册页
  reload              更改正在运行的 Caddy 实例的配置 热重载
  remove-package 删除 Caddy 包（实验）
  respond           用于开发和测试的简单、硬编码的 HTTP 响应
  reverse-proxy  快速且可用于生产的反向代理
  run                   启动 Caddy 进程在前台运行
  start                 在后台启动Caddy进程
  stop                 停止后台启动的 Caddy 进程
  storage            使用 Caddy 存储的命令（实验）
  trust                将 CA 证书安装到本地信任存储中
  untrust            卸载本地信任的 CA 证书
  upgrade          升级当前caddy二进制文件，这个过程并不会停止caddy服务（实验）
  validate           测试Caddyfile配置文件是否有错误
  version             打印caddy版本号
```

标志：
```shell
  -h, --help       caddy帮助信息
  -v, --version   caddy版本号
```

使用“caddy [命令] --help” 获取子命令的更多帮助信息。

完整文档可在以下位置获取：
https://caddyserver.com/docs/command-line

---------------------------------------------------------------------------------------
## adapt命令：

将配置调整为 Caddy 的本机 JSON 格式并写入
输出到 stdout，以及对 stderr 的任何警告。

如果指定了 --pretty，则输出将采用缩进格式
为了人类可读性。

如果使用--validate，将检查调整后的配置的有效性。
如果配置无效，则会将错误打印到 stderr 并打印非
将返回零退出状态。

如果指定了--envfile，则为带有环境变量的环境文件
KEY=VALUE格式的会被加载到Caddy进程中。

用法：
  caddy adapt [标志]

标志：
```shell
  -a, --adapter [参数]   配置适配器的名称（默认“caddyfile”）其实就是告诉命令，要使用什么样的适配器，把相应的配置转为JSON原生配置
  -c, --config [参数]     配置文件（必需，如/etc/Caddyfile）
      --envfile [参数]     要加载的环境文件
  -h, --help                   adapt帮助信息
  -p, --pretty               是否美化输出，格式化输出以方便人类阅读
      --validate              可以帮助检验下输出，其实并没有真正的运行caddy
```

------------------------------------------------------------------------------------------------
 ## add-package 命令：

下载带有指定包（模块/插件）的更新的 Caddy 二进制文件
添加。保留现有包。如果任何包是，则返回错误
已经包含在内。实验性：可能会更改或删除。

用法：
  caddy  add-package  [标志]

标志：
```shell
  -h, --help         add-package帮助信息
  -k, --keep-backup 保留备份的二进制文件，而不是删除它
```
----------------------------------------------------------------------------------------------------
## completion 命令：

加载完成：

```shell
        bash：

          $ source <(caddy completion bash)

          # 要加载每个会话的完成内容，请执行一次：
          # Linux:
          $ caddy completion bash > /etc/bash_completion.d/caddy
          ＃ 苹果系统：
          $ caddy completion bash > $(brew --prefix)/etc/bash_completion.d/caddy
```

```shell
        zsh：

          # 如果您的环境中尚未启用 shell 完成，
          # 你需要启用它。您可以执行一次以下命令：

          $ echo "autoload -U compinit; compinit" >> ~/.zshrc

          # 要加载每个会话的完成内容，请执行一次：
          $ caddy completion zsh > "${fpath[1]}/_caddy"

          # 您需要启动一个新的 shell 才能使此设置生效。
```

```shell
        fish：

          $ caddy completion fish | source

          # 要加载每个会话的完成内容，请执行一次：
          $ caddy completion fish > ~/.config/fish/completions/caddy.fish
```

```shell
        PowerShell:

          PS> caddy completion powershell | Out-String | Invoke-Expression

          # 要加载每个新会话的完成信息，请运行：
         PS> caddy completion powershell > caddy.ps1
          # 并从您的 PowerShell 配置文件中获取此文件。
```

用法：
  caddy completion [bash|zsh|fish|powershell]

标志：
  -h, --help    completion的帮助信息

----------------------------------------------------------------------------------------------
## environ 命令：

打印此 Caddy 进程所看到的环境。

环境包括系统中设置的变量。如果你的球童
配置使用环境变量（例如“{env.VARIABLE}”）然后
该命令可用于验证变量是否具有您在配置中期望的值。

如果指定了--envfile，则为带有环境变量的环境文件
KEY=VALUE格式的会被加载到Caddy进程中。

请注意，环境可能会有所不同，具体取决于您运行 Caddy 的方式。
由服务管理器启动的 Caddy 实例环境，例如
systemd 通常与从您继承的环境不同shell或terminal。

您还可以在使用“caddy run”的同时打印环境
通过添加“--environ”标志。

环境可能包含敏感数据。

用法：
  caddy environ [标志]

标志：
```shell
      --envfile [参数]     要加载的环境文件
      -h, --help               environ帮助信息
```

----------------------------------------------------------------------------------------
## file-server 命令：

一个简单但可用于生产的文件服务器。对于快速部署很有用，
演示和开发。

侦听器的套接字地址可以使用 --listen 标志进行自定义。

如果使用--domain指定域名，则默认监听地址
将更改为 HTTPS 端口，服务器将使用 HTTPS。如果使用
公共域名，确保已经正确配置好 A或AAAA 记录
再使用此选项。

如果启用 --browse，则对没有索引文件的文件夹的请求将
使用文件列表进行响应。

用法：
  caddy file-server  [标志]
  caddy file-server  [命令]

可用命令：
  export-template     导出默认文件浏览器模板

标志：
```shell
  -a, --access-log    启用访问日志
  -b, --browse        启用目录浏览
  -v, --debug          启用详细调试日志
  -d, --domain [参数] 提供文件的域名
  -h, --help              file-server帮助信息
  -l, --listen [参数]    绑定监听地址
  -r, --root [参数]     需要访问的目录路径
  -t, --templates 启用模板渲染
```
使用“caddy file-server [命令] --help”了解有关命令的更多信息。

----------------------------------------------------------------------------------
## fmt 命令：

通过添加适当的缩进和空格来格式化 Caddyfile 以改进
人类可读性，使配置文件更整齐美观。它将结果打印到标准输出。

如果加了 --overwrite参数 ，则修饰后直接写入配置文件
直接修改而不是打印出来。

如果指定了 --diff，则输出将与输入进行比较，并且
行将在不同的地方以“-”和“+”为前缀。注意
未更改的行以两个空格为前缀以进行对齐，并且这
不是有效的补丁格式。

如果您希望使用 stdin 而不是常规文件，请使用 - 作为路径。
从 stdin 读取时，--overwrite 标志无效：结果
始终打印到标准输出。

用法：
  caddy fmt [标志]

标志：
```shell
  -d, --diff     打印输入文件和格式化输出之间的差异
  -h, --help      fmt 帮助信息
  -w, --overwrite 将修饰后的直接覆盖原配置文件
```

-----------------------------------------------------------------------------------
 ## hash-password 命令：

 哈希明文密码的便捷方法。所结果的
hash 作为 base64 字符串写入 stdout。

--plaintext，当省略时，将从标准输入读取。如果
Caddy 连接到控制终端，明文将
不被附和。

--algorithm 可以是 bcrypt 或 scrypt。如果是 scrypt，则默认
使用参数。

对于需要盐的算法使用 --salt 标志
被提供（scrypt）。

请注意，scrypt 已被弃用。请改用“bcrypt”。

用法：
  caddy  hash-password [标志]

标志：
```shell
  -a, --algorithm [参数]    哈希算法的名称（默认“bcrypt”）
  -h, --help                       hash-password帮助信息
  -p, --plaintext [参数]     明文密码
  -s, --salt [参数]              密码盐
```

------------------------------------------------------------------------------------
## list-modules 命令：

列出已安装的 Caddy 模块

用法：
  caddy list-modules [标志]

标志：
```shell
  -h, --help            list-modules的帮助
      --packages     打印包路径
  -s, --skip-standard   跳过打印标准模块
      --versions        打印版本信息
```

------------------------------------------------------------------------------------------
## manpage 命令：

将 Caddy 命令的手册页生成到指定目录中
标记为第 8 节（系统管理）。

手册页文件生成到指定的目录中
--directory 的参数。如果该目录不存在，则会创建该目录。

用法：
  caddy manpage [标志]

标志：
```shell
  -o, --directory [参数]       生成联机帮助页的输出目录
  -h, --help                         manpage帮助信息
```

  -------------------------------------------------------------------------------------------
## reload 命令：
为正在运行的 Caddy 实例提供新的配置。这有同样的效果
与将文档发布到 /load API 端点一样，但对于简单的操作来说很方便
围绕配置文件的工作流程。

由于管理端点是可配置的，因此会加载端点配置
来自 --address 标志（如果指定）；否则它是从给定的加载
配置文件；否则采用默认值。

用法：
  caddy reload [标志]

标志：
```shell
  -a, --adapter [参数]  要应用的配置适配器的名称
      --address [参数]   管理侦听器的地址（如果与配置不同）
  -c, --config [参数]    配置文件（必需，绝对路径）
  -f, --force           强制重新加载配置，即使它是相同的
  -h, --help           重新加载帮助
```

-------------------------------------------------------------------------------------------------
## reverse-proxy 命令：

一个简单但可用于生产的反向代理。对于快速部署很有用，
演示和开发。

只需将 HTTP(S) 流量从 --from 地址传送到 --to 地址即可。
可以通过重复该标志来指定多个 --to 地址。

除非地址中另有指定，否则 --from 地址将为
如果给出主机名，则假定为 HTTPS，并且 --to 地址将为
假设是 HTTP。

如果 --from 地址有主机或 IP，Caddy 将尝试提供服务
使用证书通过 HTTPS 进行代理（除非被 HTTP 方案覆盖）
或端口）。

如果提供 HTTPS：
```shell
  --disable-redirects 可用于避免绑定到 HTTP 端口。
  --internal-certs 可用于强制使用内部颁发证书
    CA 而不是尝试颁发公共证书。
```
对于代理：
```shell
  --header-up 可用于设置发送到上游的请求标头。
  --header-down 可用于设置发送回客户端的响应标头。
  --change-host-header 将请求上的主机标头设置为该地址
    上游的，而不是默认传入的主机标头。
        这是 --header-up "Host: {http.reverse_proxy.upstream.hostport}" 的快捷方式。
  --insecure 禁用上游的 TLS 验证。警告：这个
    通过不验证上游的证书来禁用安全性。
```

用法：
  caddy reverse-proxy [标志]

标志：
```shell
  --access-log    启用访问日志
  -c, --change-host-header 将上游主机标头设置为上游地址
  -v, --debug 启用详细调试日志
  -r, --disable-redirects 禁用 HTTP->HTTPS 重定向
  -f, --from [参数] 接收流量的地址（默认“localhost”）
  -d, --header-down [参数] 设置响应头发送回客户端（格式：“Field: value”）
  -H, --header-up [参数] 设置发送到上游的请求标头（格式：“Field: value”）
  -h, --help      反向代理reverse-proxy帮助
      --insecure 禁用 TLS 验证（警告：不验证 TLS 证书将禁用安全性！）
  -i, --internal-certs 使用内部 CA 颁发证书
  -t, --to [参数] 流量应发送到的上游地址
```
----------------------------------------------------------------------------------------------
## run 命令：
启动 Caddy 进程，可以选择使用初始配置文件进行引导，
并无限期阻塞，直到服务器停止；即运行 Caddy
“守护进程”模式（前台）。

如果指定了配置文件，它将在进程结束后立即应用
在跑。如果配置文件不是 Caddy 的原生 JSON 格式，您可以
使用 --adapter 指定适配器以适应给定的配置文件
Caddy 的原生格式。配置适配器必须是已注册的模块。任何
警告将打印到日志中，但请注意，任何未经调整的调整
错误将立即被使用。如果您想查看结果
首先进行适应，使用“adapt”子命令。

作为一种特殊情况，如果当前工作目录有一个名为
“Caddyfile”和 caddyfile 配置适配器已插入（默认），然后
即使没有任何命令，该文件也会被加载并用于配置 Caddy
线标志。

如果指定了--envfile，则为带有环境变量的环境文件
KEY=VALUE格式的会被加载到Caddy进程中。

如果指定了 --environ ，则 Caddy 进程所看到的环境将
在开始之前打印。这与 environ 命令相同，但是
打印后不会退出，可用于故障排除。

如果有自动配置，则 --resume 标志将覆盖 --config 标志
保存存档。如果使用 --resume 并且不存在自动保存文件，则不是错误。

如果指定了--watch，配置文件将在之后自动加载
变化。 ?? 这可以使无意的配置更改变得更容易；只用这个
本地开发环境中的选项。

用法：
  caddy run [标志]

标志：
```shell
  -a, --adapter [参数]  要应用的配置适配器的名称
  -c, --config [参数]  配置文件（绝对路径）
      --envfile [参数]  要加载的环境文件
  -e, --environ   打印环境
  -h, --help  run帮助
      --pidfile [参数]  要写入进程 ID 的文件路径
      --pingback [参数]  成功时将确认字节回显到此地址
  -r, --resume 使用保存的配置（如果有）（并且优先于 --config 文件）
  -w, --watch 监视配置文件的更改并自动重新加载
```

-----------------------------------------------------------------------------------------------
## start 命令：
启动 Caddy 进程，可以选择使用初始配置文件进行引导。
该命令在服务器开始运行或运行失败后解除阻塞。

如果指定了--envfile，则为带有环境变量的环境文件
KEY=VALUE格式的会被加载到Caddy进程中。

在 Windows 上，生成的子进程将保持连接到终端，因此
关上窗户会强行阻止Caddy；为了避免忘记这一点，请尝试
使用“caddy run”代替将其保留在前台。

用法：
  caddy start [标志]

标志：
```shell
  -a, --adapter [参数]  要应用的配置适配器的文件名
  -c, --config  [参数]  配置文件（绝对路径）
      --envfile  [参数] 要加载的环境文件
  -h, --help   start帮助
      --pidfile  [参数] 要写入进程 ID 的文件路径
  -w, --watch 自动重新加载更改的配置文件 
```

-------------------------------------------------------------------------------------------
## stop 命令：
尽可能优雅地停止后台 Caddy 进程。

它要求管理 API 已启用且可访问，因为它将
使用 API 的 /stop 端点。该请求的地址可以自定义
使用 --address 标志，或者使用给定的 --config（如果不是默认值）。

用法：
  caddy stop [标志]

标志：
```shell
  -a, --adapter [参数] 要应用的配置适配器的名称（当使用 --config 时）
      --address [参数] 用于到达管理 API 端点的地址（如果不是默认地址）
  -c, --config [参数]  用于解析管理地址的配置文件（如果未使用 --address）
  -h, --help       stop帮助
```
  -------------------------------------------------------------------------------------------
 ## storage 命令：
 允许导出和导入 Caddy 的存储内容。这两个命令可以是
组合在管道中以直接从一个存储传输到另一个存储：

```shell
$ caddy  export --config Caddyfile.old --output - |
> Caddy import --config Caddyfile.new --input -
```

- 参数分别指 stdout 和 stdin。

注意：当从 file_system 存储（默认）导入或导出时，该命令
应以拥有关联根路径的用户身份运行。

实验性：可能会更改或删除。

用法：
  caddy storage [命令]

可用命令：
```shell
  export   将存储资产导出为 tarball
  import   从 tarball 导入存储资产。
```
标志：
  -h, --help   storage帮助
- - - - - - - - - - - - - - - - - - - - 
 export 参数：
配置的存储模块的内容（TLS证书等）
通过 tarball 导出。

--output 是必需的，- 可以为标准输出给出。

用法：
  caddy storage export --config <路径> --output <路径> [标志]

标志：
```shell
  -c, --config [参数]   输入配置文件（必需）
  -h, --help     export帮助
  -o, --output [参数]  输出路径
```
- - - -- - - - - - - - - - - - - - - - -
import 参数：
将存储资产导入到配置的存储模块。导入文件必须是
一个 tar 档案。

--input 是必需的，- 可以为标准输入给出。

用法：
  caddy storage import --config <路径> --input <路径> [标志]

标志：
```shell
  -c, --config [参数] 要加载的配置文件（必需）
  -h, --help import帮助
  -i, --input [参数] 要加载的资产的 Tar （必需）
```

-------------------------------------------------------------------------------
## trust 命令：
将根证书添加到本地信任存储中。

Caddy 将尝试将其根证书安装到本地
信任在第一次生成时会自动存储，但它
如果 Caddy 没有适当的权限，则可能会失败
写入信任存储。此命令是预安装所必需的
如果服务器进程作为
非特权用户（例如通过 systemd）。

默认情况下，此命令安装 Caddy 的根证书
默认 CA（即“本地”）。您可以指定另一个CA的ID
带有 --ca 标志。

此命令将尝试连接到运行于以下位置的 Caddy 管理 API
'localhost:2019' 获取根证书。您可以
显式指定 --address，或使用 --config 标志加载
如果不使用默认值，则从您的配置中获取管理地址。

用法：
  caddy trust [标志]

标志：
```shell
  -a, --adapter [参数]  要应用的配置适配器的名称（如果使用 --config）
      --address [参数]  管理 API 侦听器的地址（如果未使用 --config）
      --ca [参数] 要信任的 CA 的 ID（默认为“本地”）
  -c, --config [参数]  配置文件（如果未使用--address）
  -h, --help   trust帮助
```

-----------------------------------------------------------------------------------------------
## untrust 命令：
不信任本地信任存储中的根证书。

该命令卸载信任；它不一定会删除
完全来自信任存储的根证书。于是，反复
信任和不信任新证书可能会填满信任数据库。

此命令不会删除或修改 Caddy 的证书文件
配置的存储。

该命令可以通过以下两种方式之一使用。要么通过指定
通过证书的直接路径不信任哪个证书
带有 --cert 标志的文件，或者通过获取根证书
来自管理 API 的 CA（默认行为）。

如果使用管理 API，则 CA 默认为“本地”。您可以
使用 --ca 标志指定另一个 CA 的 ID。默认情况下，这
将尝试连接到运行于以下位置的 Caddy 管理 API
'localhost:2019' 获取根证书。
您可以显式指定 --address，或使用 --config 标志
如果不使用默认值，则从您的配置加载管理地址。

用法：
  caddy untrust [标志]

标志：
```shell
  -a, --adapter [参数] 要应用的配置适配器的名称（如果使用 --config）
      --address [参数] 管理 API 侦听器的地址（如果未使用 --config）
      --ca [参数] 要不信任的 CA 的 ID（默认为“本地”）
  -p, --cert [参数] 要不信任的 CA 证书的路径
  -c, --config [参数] 配置文件（如果未使用--address）
  -h, --help   untrust帮助
```

---------------------------------------------------------------------------------
## upgrade 命令： 
下载最新的 Caddy 二进制文件，其中包含相同的模块/插件
最新版本。实验功能：可能会更改或删除。

用法：
  caddy upgrade [标志]

标志：
```shell
  -h, --help   upgrade帮助
  -k, --keep-backup 保留备份的二进制文件，而不是删除它
```

  --------------------------------------------------------------------------------
## validate 命令：

加载并配置提供的配置，但不开始运行它。
这是一个验证Caddyfile配置文件的命令，它会模拟启动caddy，
但是并不会真的启动。验证的过程中，遇到的问题，会在控制台输出。
它的使用和 adapt 命令基本上一致。

如果指定了--envfile，则为带有环境变量的环境文件
KEY=VALUE格式的会被加载到Caddy进程中。

用法：
  caddy validate [标志]

标志：
```shell
  -a, --adapter [参数] 配置适配器的名称
  -c, --config [参数] 输入配置文件
      --envfile [参数] 要加载的环境文件
  -h, --help    validate验证
```
