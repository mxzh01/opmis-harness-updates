# 更新源仓库 —— 一次性配置，约 3 分钟

程序里已经写死了下面三个地址（按顺序尝试，任何一个通就生效）：

```
1. https://mxzh01.github.io/opmis-harness-updates/update.json          ← 缓存约 10 分钟，优先
2. https://raw.githubusercontent.com/mxzh01/opmis-harness-updates/main/update.json   ← 缓存约 5 分钟
3. https://cdn.jsdelivr.net/gh/mxzh01/opmis-harness-updates@main/update.json        ← 国内兜底，缓存较长
```

**仓库名必须是 `opmis-harness-updates`，文件必须是 `update.json`，放在仓库根目录，分
支必须是 `main`。** 名字不对地址就 404，程序会静默忽略（不报错，但伙伴收不到提醒）。

---

## 你需要做的三步

### 第 1 步：建仓库

打开 https://github.com/new

- Repository name：`opmis-harness-updates`
- 选 **Public**（必须是公开，私有仓库这两个地址读不到）
- 勾选 **Add a README file**
- 点 **Create repository**

### 第 2 步：上传 update.json

在该仓库页面点 **Add file → Upload files**，
把本文件夹里的 `update.json` 拖进去，点 **Commit changes**。

### 第 3 步：开启 GitHub Pages

仓库 **Settings → Pages**：

- Source：`Deploy from a branch`
- Branch：`main`，目录选 `/ (root)`
- 点 **Save**，等约 1 分钟

做完这三步，三个地址里第 1 个就活了。

---

## 验证是否配置成功

在浏览器打开：

```
https://raw.githubusercontent.com/mxzh01/opmis-harness-updates/main/update.json
```

能看到 JSON 内容就说明成了。如果打不开，用这个试：

```
https://cdn.jsdelivr.net/gh/mxzh01/opmis-harness-updates@main/update.json
```

（`raw.githubusercontent.com` 在国内经常被墙，这是正常的，程序会自动走其它两个地址。）

---

## 以后每次发新版本怎么更新

1. 把新的安装包传到该仓库的 **Releases**（新建 tag，例如 `v1.2.4`）
2. 编辑仓库里的 `update.json`，改成：

```json
{
  "version": "1.2.4",
  "url": "https://github.com/mxzh01/opmis-harness-updates/releases/download/v1.2.4/OPMIS-Harness-Setup-1.2.4.exe",
  "notes": "本次更新说明，可以写多行"
}
```

3. 提交。**已装旧版的伙伴最迟第二天启动时就会看到中文更新提示。**

### 字段说明

| 字段 | 必填 | 说明 |
|---|---|---|
| `version` | 是 | 最新版本号。必须比伙伴装的版本大才会提示 |
| `url` | 否 | 点「立即下载」时打开的链接。留空则只显示「知道了」 |
| `notes` | 否 | 更新说明，会显示在提示框里，支持 `\n` 换行 |

---

## 注意

- **`url` 可以指向任何地方**，不一定非要是 GitHub。144MB 的安装包放在 GitHub
  Releases 国内下载会比较慢，如果你有阿里云 OSS / 腾讯云 COS / 公司服务器，
  把链接换掉即可，随时可改，不需要重新打包程序。
- **只有 `update.json` 这 200 字节是必须托管的**，安装包放哪都行。
- **不要把这个仓库设为私有**，否则程序读不到。
- 程序只会**提示**，不会自动下载安装。下载链接会用系统浏览器打开，由用户自己决定。
