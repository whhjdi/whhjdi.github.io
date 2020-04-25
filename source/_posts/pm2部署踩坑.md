---
title: pm2部署踩坑
date: 2019-04-04 11:48:12
tags: [pm2]
cover_img: https://ws1.sinaimg.cn/large/006tKfTcly1g1qh7zrvaaj31560gc41t.jpg
feature_img:
description:
keywords: [pm2]
---

# pm2 和 nginx

## nginx 配置

安装好之后，需要配置一下配置文件
在`/etc/nginx/conf.d`下新建一个配置文件,具体配置如下

```bash
upstream weapp {
  server 127.0.0.1:3000;
}

server {
  listen 80;
  server_name 59.110.237.228;

  location / {
    proxy_pass http://weapp;
    proxy_set_header Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Nginx-Proxy true;
    proxy_redirect off;
  }
}
```

由于我的域名是腾讯的，服务器是阿里的，所以无法把域名解析到阿里云，只能通过服务器的 ip 地址访问

## pm2

本地新建一个项目，添加`ecosystem.json`,具体配置如下

```json
{
	"apps": [
		{
			"name": "weapp",
			"script": "server.js",
			"env": {
				"COMMON_VARIABLE": "true"
			},
			"env_production": {
				"NODE_ENV": "production"
			}
		}
	],
	"deploy": {
		"production": {
			"user": "root",
			"host": "59.110.237.228",
			"ref": "origin/master",
			"repo": "git@github.com:whhjdi/weapp.git",
			"path": "/www/weapp/production",
			"ssh_options": "StrictHostKeyChecking=no",
			"pre-deploy-local": "echo 'This is a local executed command'",
			"env": {
				"NODE_ENV": "production"
			}
		}
	}
}
```

然后 push 到 github 上
把服务的 pm2 先杀掉`pm2 delete all`
在服务器相应的目录新建项目文件夹，赋予权限`sudo chmod 777 xxx`
本地执行`pm2 deploy ecosystem.json production setup`
success 之后执行`pm2 deploy ecosystem.json production`
然后`pm2 list`就可以查看启动的 server 了

### 坑

1. 无密码连接服务器，需要把本地的密钥拷贝的服务器相应的文件
   `scp ~/.ssh/id_rsa.pub root@47.98.154.75:/root/.ssh/authorized_keys`

2. 远程部署的时候显示找不到 pm2 命令，需要在服务器建立 link
   `whereis pm2`，显示路径为 xxx
   `ls -s xxx /usr/local/bin/pm2`

3. 部署的时候卡住，找了好的资料都没发现解决方案，终于
   把下边的放到`~/.bashrc`头部

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

然后`source ~/.bashrc`

重新运行
`pm2 deploy ecosystem.json production`
