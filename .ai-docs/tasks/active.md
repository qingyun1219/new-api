## Token 激活后有效期调整
创建于：2026-05-04_20:07:06

### 任务描述
调整卡密/Token 的到期语义：创建时保持永不过期，首次真实消费后才开始按有效期计时，并同步更新新版与旧版前端展示。

### 分析
- 后端 `model/token.go` 新增 `ActivatedTime` 和 `ValidDurationSeconds`，`Token.Update()` 也同步持久化这两个字段。
- `controller/token.go` 在创建时将带有效期的 token 置为 `expired_time = -1`，避免未激活阶段提前过期。
- 首次真实消费路径中通过原子更新激活 token，并在同一事务内写入 `activated_time` 和 `expired_time`。
- 新版前端 `web/default/src/features/keys` 已将表单字段从固定过期时间改为“激活后有效期”，列表中增加未激活提示和激活时间展示。
- 旧版前端 `web/classic/src/components/table/tokens` 已同步改为展示激活后有效期、激活时间，以及“未激活”状态提示。
- 替换式运行文件需要先构建 `web/default/dist` 和 `web/classic/dist`，再由 `main.go` 通过 `go:embed` 一起嵌入 Linux 可执行文件。
- 旧版前端样式回归定位为 `vite-plugin-semi` 的 `cssLayer` 配置导致样式层级异常；已关闭该选项并重新打包。

### 提议的解决方案
当前实现已按确认方案落地，不再分支比较。

### 任务进度
- 2026-05-04_20:07:06：完成后端模型、接口、首次消费激活逻辑，以及新版/旧版前端同步。
- 2026-05-04_20:07:06：完成静态检查与编译级验证；前端本地检查命令受当前环境缺少 `tsc` / `prettier` 可执行文件影响，未能在该环境直接跑通。
- 2026-05-04_20:33:01：补充 `/api/usage/token` 外部查询接口返回字段：`activated_at` 和 `activated_expires_at`。
- 2026-05-04_21:58:35：使用 npm 构建新版与旧版前端 `dist`，并编译 `GOOS=linux GOARCH=amd64 CGO_ENABLED=0` 的可替换运行文件。
- 2026-05-04_23:03:22：关闭旧版前端 `vite-plugin-semi` 的 `cssLayer`，重新构建并覆盖交付产物。

### 最终审查
实现与已确认方案一致；未激活 token 在首次真实消费前保持永不过期，前后端展示已同步调整。旧版前端样式回归已通过关闭 `cssLayer` 修复，交付产物 `D:\\Temp\\cursor\\new-api\\new-api` 已包含两套前端静态资源，压缩包为 `D:\\Temp\\cursor\\new-api\\new-api-linux-amd64.zip`。