# Screen Operations

## Rule

将 screen 视为长任务的强制进程托管方式。

## Start Policy

- 必须指定显式 session name
- 必须开启持久化日志输出
- 启动前将元数据写入文件

## Session Lifecycle

```bash
# 启动（detached + log）
screen -dmS <session> -L -Logfile <log-file> bash -c '<command>'

# 检查状态
screen -ls | grep <session>

# 附着
screen -r <session>

# 停止
screen -S <session> -X quit
```

## 需要 sudo 的场景

某些服务器上 screen session 在 root 下运行：

```bash
sudo screen -ls
sudo screen -r <session>
sudo screen -S <session> -X quit
```

## Failure Recovery

- 会话退出时，先检查日志末尾内容
- 使用相同的运行元数据和命令重新启动
- 不要启动第二个同名的冲突会话
