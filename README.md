# Linux 自动巡检系统

## 1. 问题（Problem）

单服务器运维时，人工巡检耗时且容易遗漏，故障发现滞后。
- 人工登录多台服务器逐条检查，单次巡检需 10-15 分钟
- 检查项不统一，容易遗漏磁盘、内存等关键指标
- 故障依赖用户上报，无历史数据追溯

## 2. 方案（Solution）

六函数模块化 Shell 脚本，覆盖 **CPU / 内存 / 磁盘 / 服务 / 网络 / 日志** 六维指标：

| 检查项 | 函数名 | 阈值 / 行为 |
|---|---|---|
| CPU | `check_cpu()` | 使用率 &gt; 80% 告警 |
| 内存 | `check_mem()` | 剩余 &lt; 200MB 告警 |
| 磁盘 | `check_disk()` | 使用率 &gt; 80% 告警 |
| 服务 | `check_nginx()` / `check_mysql()` / `check_apache()` | 按节点差异化检查 |
| 网络 | `check_mysql_conn()` | 远程连接可用性检测 |
| 日志 | `archive_logs()` | 7 天前自动压缩归档 |

巡检数据自动写入 **MySQL（ops_db.server_info）**，支持历史趋势查询。  
日志自动归档到 `~/logs/`，保留 7 天。

## 3. 验证（Verification）

- **部署节点**：db-srv（192.168.229.136）+ web-srv（192.168.229.135）
- **执行方式**：cron 定时 8:00 / 20:00
- **脚本路径**：`/home/yuwei/scripts/check_services_v3.sh`
- **工程化规范**：所有脚本通过 ShellCheck 零 error，统一注释头（Author/Description/Version/Usage）

## 4. 效果（量化）

| 指标 | 数字 |
|---|---|
| 替代人工巡检，单次执行时间 | **&lt; 1 秒（实测 0.285s）** |
| 日志归档策略 | **7 天自动压缩，当前磁盘占用 152K** |
| 无人值守错误防护 | **`set -euo pipefail`，错误不静默传播** |
| 巡检频率 | **每日 2 次（cron 自动触发，零人工干预）** |

## 5. 技术栈

- Bash / Shell
- MySQL
- cron
- rsync（异机备份）

## 6. 目录结构
.
├── scripts/
│   └── check_services_v3.sh    # 六函数巡检主脚本
├── docs/
│   ├── README.md
│   └── SOP.md
└── ...

## 7. 关键设计

- **模块化六函数**：每个检查项独立函数，故障定位精确到维度
- **节点差异化**：db-srv 检查 Nginx + MySQL，web-srv 检查 Nginx + Apache + PHP-FPM
- **数据持久化**：巡检结果写入 MySQL，支持跨节点历史查询
- **工程化规范**：统一函数命名（`check_xxx` / `backup_xxx` / `archive_xxx`），统一注释头
