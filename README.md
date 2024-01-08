# 项目地址：https://github.com/caddyserver/caddy

添加了两个插件：

1. https://github.com/mholt/caddy-webdav
 
2. https://github.com/aksdb/caddy-cgi


启动方法参照hiboy：https://github.com/hiboyhiboy/opt-script/blob/0dcfdf2911f052218ffb1b7cd220f1457249f538/script/Sh87_cad_dy.sh#L151C28-L151C78

/tmp/caddy run --config /tmp/Caddyfile --adapter caddyfile

Caddyfile 配置文件内容如下：

{ # 全局配置
order cgi before respond # 启动 cgi 模块 # 全局配置
order webdav before file_server # 启动 webdav 模块 # 全局配置
admin off # 关闭 API 端口 # 全局配置
} # 全局配置

:8889 {
 #文件服务器，后面是文件路径；效果类似于https://opt.cn2qq.com 
 root * /mnt/sda1/filebrowser/caddy
 file_server browse
 
 #在线打开文本显示中文字符不乱码
header {
                Content-Type "text/plain; charset=utf-8"
        }
        
 log {
  output file /tmp/caddy/requests.log {
   roll_size     1MiB
   roll_local_time
   roll_keep     5
   roll_keep_for 120h
  }
 }
}

 :12322 {
  webdav * {
   root /mnt/sda1/caddy/www
  }
 }




 ------------------------------------------------------------------------------
 #另外的功能 我没用过
 
 #认证，账号admin 密码123456
  basicauth /dav/* {
   admin $2a$14$RdbOHzJhf5BaapSdlYTCbe.yWY9cEZjyDpfgwStY28K/qsM1tX8tu
  }
  #webdav
  webdav /dav/* {
   prefix /dav
   root /
  }
  
#ssl证书就是
tls {
  protocols tls1.2 tls1.3
 } 

-----------------------------------------------------------------------------
#basicauth的密码生成是直接用caddy命令：
/tmp/caddy hash-password  --plaintext 123456
