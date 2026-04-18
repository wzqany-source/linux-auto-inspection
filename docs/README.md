# 自动巡检项目 (Auto-Inspection)

## 项目背景
自动化巡检服务器核心服务状态，包括 Nginx、MySQL、Apache、磁盘、内存、CPU。
替代人工手动检查，提高效率，统一告警标准。

## 运行环境
- 服务器：db-srv (192.168.229.136)
- 系统：CentOS 7+
- 依赖：bash, nginx, mysql, find, gzip

## 功能列表
1. check_nginx() - 检查 Nginx 服务状态
2. check_mysql() - 检查 MySQL 服务状态
3. check_apache() - 检查 Apache 服务状态
4. check_disk() - 检查磁盘使用率
5. check_mem() - 检查内存使用率
6. check_cpu() - 检查 CPU 使用率
7. archive_logs() - 归档 7 天前的日志

## 使用方法
```bash
# 手动执行
bash /home/yuwei/projects/auto-inspection/scripts/check_services_v3.sh

# 查看定时任务
crontab -l


##目录结构
/home/yuwei/projects/auto-inspection/
├── scripts/      # 脚本文件
├── logs/         # 运行日志
└── docs/         # 项目文档

##版本记录## 版本记录
- v1.0 - 基础巡检功能
- v2.0 - 五函数版本
- v3.0 - 新增 Apache 检查和日志归档
