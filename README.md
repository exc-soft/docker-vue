### 基于 Docker-compose 的 Vue CLI3 镜像

使用方式

    git clone https://github.com/exc-soft/docker-vue.git

    cd docker-vue
    docker-compose up -d --build

项目目录

如果你有自己的项目就可以放到 `/home/node/app/` 下面，没有的话则可以在 app 目录下创建项目，例如执行 `vue create seller`，则会创建 `/home/node/app/seller` 项目目录。

#### 访问配置

自定义端口可在项目目录下编辑或创建 `vue.config.js` 配置以下代码

    module.exports = {
        devServer: {
            host: '0.0.0.0',
            port: 3000,
            public: 'seller.sdxstore.com'
        }
    }

结合 Nginx 反向代理至 Vue 容器，如下为 nginx config，注意如果你用的云主机如果前端服务器和代理服务器不在同一个主机务必在安全组配置 3000 端口对外允许访问

    server {

        listen   80;
        server_name seller.sdxstore.com;

        location / {
            proxy_pass   http://docker宿主机的IP:3000/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }
    }