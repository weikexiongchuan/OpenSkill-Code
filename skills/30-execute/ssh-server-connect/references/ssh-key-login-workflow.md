# SSH Key Login Workflow

## First-Time Onboarding

1. 确定登录模式，记录到项目 workspace 的 `servers/{alias}/state.md` 和 `login/method.md`。
2. 若 `LOGIN_METHOD=key`，本地密钥不存在则创建。
3. 若平台支持 SSH 密钥管理（如云平台控制台），优先在平台设置中上传公钥。
4. 若平台不支持密钥导入，使用一次性密码登录推送公钥。
5. 验证 `BatchMode` key 登录。
6. 在本地 SSH config 中添加 host alias。

## Password Mode

当凭据动态变化且 key 模式未配置时使用。

```bash
ssh -p <port> <user>@<host> '<command>'
scp -P <port> <local_path> <user>@<host>:<remote_path>
```

不要在 skill 文件中存储明文密码。

## Key Mode

推荐用于重复自动化和稳定操作。

1. 本地密钥不存在则创建。
2. 优先通过平台设置上传公钥。
3. 平台不支持时通过密码登录推送公钥。
4. 验证 `BatchMode` key 登录。
5. 添加本地 SSH config entry。

## Recommended SSH Config Entry

```sshconfig
Host <alias>
  HostName <host>
  Port <port>
  User <user>
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes
  ServerAliveInterval 30
  ServerAliveCountMax 3
```

## Verification Command

```bash
ssh -o BatchMode=yes -p <port> <user>@<host> 'echo key-login-ok'
```

## SSH Tunnel (Background)

```bash
ssh -f -N -L <local_port>:<remote_host>:<remote_port> -p <port> <user>@<host>
```

验证隧道：
```bash
curl -s http://127.0.0.1:<local_port>/
```
