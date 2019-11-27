1.docker安装mysql5.7
```
docker pull centos/mysql-57-centos7 
```
2.docker运行
```
sudo docker run --name mysql3306 -p 3306:3306  -e MYSQL_ROOT_PASSWORD=123456 centos/mysql-57-centos7:latest

```