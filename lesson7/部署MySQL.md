# 部署MySQL
## 需求
在容器部署MySQL，并通过外部客户端操作MySQL Server

## 步骤
1. 搜索mysql镜像
   ```
   docker search mysql
   ```
2. 拉取mysql镜像
   ```
   docker pull mysql:5.6
   ```
3. 创建容器，设置端口映射、目录映射
   ```
   mkdir ~/mysql

   cd ~/mysql

   docker run -id \
      -p 3307:3306 \
      --name=mysql \
      -v $PWD/conf:/etc/mysql/conf.d \
      -v $PWD/logs:/logs \
      -v $PWD/data:/var/lib/mysql \
      -e MYSQL_ROOT_PASSWORD=123456 \
      mysql:5.6

   ```
4. 操作容器的MySQL