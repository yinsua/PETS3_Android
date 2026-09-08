# PETS3 搭配速记 · 内容源

安卓 app「搭配速记」的热更新源。app 每次启动会静默读一次 `version.json`，
版本号比本机高就下载新的页面文件，下次启动生效。刷卡进度不受影响。

## 目录

```
version.json          更新清单（app 读的就是它）
content/index-vN.html 实际的页面内容
```

## version.json 字段

| 字段 | 说明 |
|---|---|
| `version` | 整数。比 app 本机版本大才会触发更新 |
| `url` | 页面文件路径，相对于 version.json 所在目录 |
| `sha256` | 页面文件的 SHA-256，校验不过就不更新 |
| `notes` | 更新说明，会显示在 app 顶部 |

## 发新版本

1. 把新页面放成 `content/index-v3.html`（**换文件名**，避开 CDN 缓存）
2. 改 `version.json`：`version` +1，`url` 指向新文件，`sha256` 换成新的
3. `git push`

算 sha256：

```bash
sha256sum content/index-v3.html
```

## app 里怎么填

设置 → 更新源 → 填 `你的GitHub用户名/仓库名`（例如 `yinsua/pets3-cards`）→ 保存并检查。

app 会依次尝试 raw.githubusercontent.com 和 jsDelivr CDN，哪个通用哪个。
raw 通常几秒内就能拿到新版；jsDelivr 对分支引用有约 12 小时缓存，
想让它立刻生效，push 后访问一次：

```
https://purge.jsdelivr.net/gh/用户名/仓库名@main/version.json
```

## 出问题了

app 里「设置 → 恢复到内置版本」可以退回随 APK 打包的那一版，重启生效。
