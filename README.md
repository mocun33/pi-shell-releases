# pi-shell-releases

本仓是 pi-shell（Windows x64 Tauri 壳）的**更新发布位**——`manifest.json` 是更新清单，GitHub Release 资产是被更新的二进制。

## 清单 schema

`{version, url, sha256, notes?}`：`version` 是远端版本号，`url` 是被更新二进制的下载地址，`sha256` 是该二进制的 SHA-256（小写十六进制），`notes` 可选说明。`version` 用点分数字段比较，远端严格大于本机才更新（本机版本从壳自身读取）。

## 消费方

pi-shell 内置更新器读取本清单：通过壳根 `update-config.json` 的 `manifestUrl`，或 env `PI_SHELL_UPDATE_MANIFEST`（env 非空优先）。**更新器不发鉴权头**，所以本仓必须保持 public。

## test-fixtures/

`test-fixtures/` 里是**故意构造的负例清单**（当前只有 `manifest-badsha.json`，sha256 全 A，用于验证「sha256 不符则拒装并回滚」），不是可用发布，请勿当作更新源使用。

## 版本与自报一致性

- `v0.1.1`：标签为 `v0.1.1`，但资产是未重编译的 `0.1.0` 二进制（sha256 与当时在役 `Pi.exe` 逐字节一致）；该标签仅用于打通更新链，**存在标签与自报版本偏差**。
- `v0.1.2` 起：资产均为当次重编译产物，exe 内 Win32 VERSIONINFO 与 `Cargo.toml`/`tauri.conf.json` 版本、标签版本三者一致（`0.1.2`、`0.1.3` 已核）。
- `v0.1.3`：更新器清单抓取改用 `curl -f`（HTTP >=400 视为失败），修掉「5xx 后重试成功但失败响应体已污染 stdout」的缺陷；下载路径保持不加 `-f`（有 sha256 校验兜底）。
