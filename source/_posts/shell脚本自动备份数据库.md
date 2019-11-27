## 利用crontab 备份mysql数据库：
创建backmysql，文件内容：
```
 user=""
 password=""
 host=""
 db_name=""
# 其它
 backup_path="/path/to/your/home/_backup/mysql"
 date=$(date +"%d-%b-%Y")
# Dump数据库到SQL文件
 mysqldump --user=$user --password=$password --host=$host --default-character-set=utf8 --hex-blob $db_name > $backup_path/$db_name-$date.sql
# 设置导出文件的缺省权限
 umask 177
# 删除30天之前的就备份文件
 find $backup_path/* -mtime +30 -exec rm {} \;
```
>// 加入执行权限
```
chown +x backmysql
```
>crontab -e 加入执行事件
(第一次运行 会出现选择什么编辑器 这里我选择vim)

```
*/1 * * * * /home/ubuntu/sql/backmysql 
>> /home/ubuntu/sql/log.txt 
>/dev/null 2>&1
```
备注：>/dev/null 2>&1	加入这条可以不安装邮件服务器 


