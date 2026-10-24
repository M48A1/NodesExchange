# [在线制作](https://m48a1.github.io/NodesExchange/Convetert4clash.html)


# NodesExchange · 节点转 Clash

一个零依赖、纯前端的节点分享链接转换工具。将多条节点链接转换为 Clash / Mihomo 配置中的 `proxies:` 节点段，也可以生成包含手动选择代理组的完整 YAML 配置。

节点链接在浏览器本地解析，无需登录，无需部署转换后端。页面不上传输入内容，也不使用本地持久化存储保存节点。

## 快速使用

### 本地打开

1. 下载本仓库中的 [Convetert4clash.html](./Convetert4clash.html)。
2. 使用现代浏览器打开下载的 HTML 文件。
3. 每行粘贴一条节点分享链接。
4. 点击「转换配置」。
5. 选择输出模式，复制结果或下载 YAML 文件。

也可以点击「试用示例」查看输出格式。示例使用占位地址和密码，不能用于连接。

如果浏览器限制本地文件的剪贴板访问，工具会选中输出内容；按 `⌘C`（macOS）或 `Ctrl+C`（Windows / Linux）手动复制即可。

### GitHub Pages

1. 在仓库中打开 **Settings → Pages**。
2. 将 **Source** 设为 **Deploy from a branch**。
3. 选择 `main` 分支和 `/(root)` 目录，点击 **Save**。
4. 等待 GitHub Pages 部署完成后，访问：

   [打开节点转换器（需先开启 Pages 并完成部署）](https://m48a1.github.io/NodesExchange/Convetert4clash.html)

当前入口文件名为 `Convetert4clash.html`，访问地址需包含该文件名。若希望访问 `https://m48a1.github.io/NodesExchange/` 就直接打开工具，可将入口文件改名为 `index.html`，并同步更新本文链接。

## 支持范围

| 协议 | 分享链接 | 当前支持 |
| --- | --- | --- |
| Shadowsocks | `ss://` | 常见 Base64 认证信息、整段 Base64 分享格式；不支持插件 |
| VMess | `vmess://` | Base64 JSON 分享格式；TCP、WebSocket、gRPC |
| VLESS | `vless://` | TCP、WebSocket、gRPC；TLS、Reality 常见参数 |
| Trojan | `trojan://` | TLS；TCP、WebSocket、gRPC |
| Hysteria2 | `hysteria2://`、`hy2://` | 密码、SNI、ALPN、证书校验选项及 Salamander 混淆 |

VLESS、Reality、Hysteria2 请使用 **Mihomo（Clash Meta）内核**。其他协议和参数是否可用，也取决于客户端内核版本与服务端设置。

支持常见的节点名称、URL 转义和 UTF-8 Base64 解码。重名节点会自动追加编号；与部分内置策略名称冲突时也会调整名称。

## 两种输出模式

### proxies 节点段

下载文件名：`clash-proxies.yaml`。

用于填入已有配置。请替换或合并现有的 `proxies:` 内容，不要在同一份配置中重复添加顶层 `proxies:` 字段。

现有 `proxy-groups:` 还需要引用对应的节点名称，节点才会出现在该代理组中。

### 完整配置

下载文件名：`clash-config.yaml`。

包含：

- 本地混合代理端口 `7890`；
- `allow-lan: false`；
- 规则模式及 `info` 日志级别；
- 成功转换的节点；
- 包含节点及 `DIRECT` 的手动选择代理组；
- 将流量交给该代理组的 `MATCH` 规则。

导入支持相应协议的客户端后，在代理组中选择节点；选择 `DIRECT` 则直接连接。此模式是基础配置，未提供自动分流规则、DNS 定制或 TUN 设置。

输出使用 YAML 块结构搭配 JSON 风格的行内对象；这种写法可用于 YAML 配置，字符串中的引号等字符会自动转义。

## 错误提示与限制

- 每行只接受一条节点分享链接，不抓取 HTTP / HTTPS 订阅，也不解析整份 Base64 订阅。
- 暂不支持 SSR、TUIC、WireGuard 等其他协议。
- 暂不支持 SS 插件、XHTTP、HTTP/2、mKCP 等传输或扩展；Reality 的 `spx` 参数也暂不支持。
- 支持表列出的是常见格式，并不代表覆盖全部分享链接方言或参数组合。
- 工具对端口、UUID、部分参数和编码进行基础检查；不替代内核配置校验。
- 部分错误会按行提示，成功的节点仍会生成输出。下载前请检查成功数量和错误信息。
- 不提供节点测速、可用性检测，也不会验证密码、公钥、加密算法或全部参数组合是否与服务器匹配。
- 转换成功表示已生成配置内容，不代表节点一定能够连接。

## 隐私

转换过程中没有调用第三方转换接口，也没有加载外部脚本、字体或统计服务。访问托管网页时，仍会产生网站托管服务的正常访问请求；节点解析在浏览器中完成。

生成的 YAML 包含服务器地址、密码或 UUID 等节点认证信息。请勿将真实节点链接或生成的配置提交到公开仓库、Issue 或截图中。

## 文件结构

```text
NodesExchange/
├── Convetert4clash.html   # 页面、样式和转换逻辑
└── README.md             # 使用与发布说明
```

无需安装依赖或运行构建命令。修改 HTML 文件即可更新工具；使用 GitHub Pages 时，提交后等待部署完成。

## 参考

- [Mihomo 配置文档](https://wiki.metacubex.one/config/proxies/)
