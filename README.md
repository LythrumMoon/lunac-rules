# 规则仓库（Registry）

「阅读」插件的公开规则仓库：把 **Kazumi / Venera / uzVideo** 三个上游生态的索引聚合成一份 `index.json`。

本仓库只发布**索引与自建来源**，不复制上游规则正文。

## 使用（插件内）

打开阅读插件 →「来源」→「导入规则」，URL 保持默认即可：

- jsDelivr（默认）：
  `https://cdn.jsdelivr.net/gh/LythrumMoon/lunac-rules@main/index.json`
- GitHub raw：
  `https://raw.githubusercontent.com/LythrumMoon/lunac-rules/main/index.json`

导入后：

1. 上游条目会显示名称 / id / 版本 / 许可，但**只登记地址，不执行**。
2. 在「聚合仓库」里找到 `Project Gutenberg（演示）`，点「安装」；
   或在导入页把 `sources/demo-gutendex.json` 全文粘进去。
3. 在总览页输入英文关键词（如 `alice`），回车即可看到公版书目与作品摘要。

> 演示来源只使用 **Gutendex / Project Gutenberg 的公版书目元数据与摘要**，不下载、不分发作品正文。
> `rateLimit` 设为 2 rps / burst 3，用于演示来源级限速。

## 目录

```
index.json            # 生成的聚合结果（导入入口）
upstream/
  kazumi.json         # 上游索引快照
  venera.json
  uzvideo.json
sources/              # 本仓库自建来源（规则内嵌，可直接安装）
  demo-gutendex.json
config.example.json   # 生成配置示例（无私有地址）
```

## 生成

索引由 Lunac 主仓库维护：

```bash
cd resource-hub
npm run build          # 先生成 SDK dist
cp registry/config.example.json registry/config.json
# 把三份上游索引放到 registry/upstream/
npm run registry:build
```

输出：`registry/index.json`（`{ rsp: 1, generatedAt, entries[] }`），再发布到本仓库根目录。

## 条目里保留什么

- `sourceUrl`：上游规则 / 脚本地址 —— **只链接，不复制内容**。
- `license` / `attribution`：**必须保留**；MIT / GPL 都要求保留版权与许可声明。
- `warnings`：不支持的能力、许可以及上游作者信息。
- `source`：**仅限 `upstream: "manual"` 的自建条目**，用于把本仓库自有规则内嵌进
  `index.json`，实现「导入 registry → 点安装」；上游条目一律不带 `source`。

## 不允许

- ❌ 为了「看起来是自己的」而删除上游的版权、许可、作者声明（MIT / GPL 明确要求保留）。
- ❌ 把 Venera / uzVideo 的脚本正文复制进本仓库（Venera 为 GPL-3.0，uzVideo 未见 LICENSE）。
- ❌ 把 `KazumiRules` 的第三方规则打包进发行物；它们由用户自行导入。
- ❌ 在 App Store Lite 构建里启用本仓库或任何远程规则导入。

> 「去除声明」在这里的正确含义只能是：**统一字段 / 去重 / 去掉重复的元数据**，
> 不是删除版权与许可信息。两者不能混为一谈。
