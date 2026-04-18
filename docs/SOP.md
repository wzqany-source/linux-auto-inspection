# 自动巡检标准操作程序 （SOP）

##1.手动执行巡检程序
---bash
bash /home/yuwei/projects/auto-inspection/scripts/check-services-v3.sh

##2.查看定时任务
crontab -l


##3常见故障处理
nginx停止---------systemctl start nginx
MySQL停止---------systemctl start mysql
磁盘满------------df-h清理大文件
日志归档失败------检查~/backup权限

##日志位置
巡检日志  ~/logs/check—services.log
归档日志 ~/backup/logs/
