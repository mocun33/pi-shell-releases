# pi-shell-releases

本仓是 pi-shell（Windows x64 Tauri 壳）的**更新发布位**——`manifest.json` 是更新清单，GitHub Release 资产是被更新的二进制。

## 清单 schema

`{version, url, sha256, notes?}`：`version` 是远端版本号，`url` 是被更新二进制的下载地址，`sha256` 是该二进制的 SHA-256（小写十六进制），`notes` 可选说明。`version` 用点分数字段比较，远端严格大于本机才更新（本机版本从壳自身读取）。

## 消费方

pi-shell 内置更新器读取本清单：通过壳根 `update-config.json` 的 `manifestUrl`，或 env `PI_SHELL_UPDATE_MANIFEST`（env 非空优先）。**更新器不发鉴权头**，所以本仓必须保持 public。

## test-fixtures/

`test-fixtures/` 里是**故意构造的负例清单**（当前只有 `manifest-badsha.json`，sha256 全 A，用于验证「sha256 不符则拒装并回滚」），不是可用发布，请勿当作更新源使用。

## 已知偏差

首个发布标签为 `v0.1.1`，但发布二进制内部自报版本仍是 `0.1.0`（未重编译，sha256 与在役 `Pi.exe` 逐字节一致）；仅版本号抬升以打通完整更新链。
