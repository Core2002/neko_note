# Neko Note

个人技术笔记，记录平时写代码、折腾服务器时踩过的坑和查过的答案。这个仓库最初只是本地一摞散落的 Markdown，写多了才整理成现在的样子——没有刻意追求体系完整，更像是按主题归档的速查本。内容大多是「当时的我」验证过的结论，可能过时，用到生产环境前请自行确认。

## 开发笔记

覆盖构建工具 (Maven、Gradle 的镜像源、打包、私服配置等)、常用语言速查 (Java、Go、Rust、Python、TypeScript 等)、以及写代码常翻的周边 (Git 批量整理、MongoDB、Flutter、LaTeX)。这类内容篇幅最大，分门别类记在 [developer.md](developer.md)。

## 运维笔记

装系统、起服务时的场景速查:Docker 下几十种常用服务的一键部署 (GitLab、Gitea、Nexus、Redis、WireGuard、RustDesk、Ollama 等)、npm / pip / conda 等国内镜像源配置、以及 Linux / Windows 的零散维护操作。都在 [operations.md](operations.md)。

## 安全笔记

记录安全工具的使用方式与参考清单:Hydra 爆破 SSH 的命令参数、WLAN 握手包破解流程，以及一份 100 个经典安全工具的功能索引 (Nessus、Wireshark、Metasploit…)。见 [cyber_security.md](cyber_security.md)。

## AI 笔记

AI 应用层的折腾记录：模型格式转换 (TensorFlow → ONNX → NCNN)、Stable Diffusion 的 ControlNet 插件安装，以及调试时保存的提示词示例。如果网络受限，记得先看 [运维笔记里的镜像配置](./operations.md#huggingface)。整理在 [artificial_intelligence.md](artificial_intelligence.md)。

## 怎么记、怎么找

- 一条经验就是一个小节，直接追加到对应分类文件末尾，不设额外归档结构。
- 引用外部资料时，统一用引用块把来源链接挂在条目末尾。
- 高频复用、容易遗忘的命令优先收录，并放成能直接复制的代码块。
- 想不起来某条在哪时，别翻目录了——用页面右上角的搜索框全文查找更快。

<details>
<summary>本站是怎么搭的 (点击展开)</summary>

内容以 Markdown 存放在 `docs/` 目录，用 **MkDocs + Material 主题**渲染成静态站。push 到仓库后，由 GitHub Actions 里的 `ci.yml` 自动执行 `mkdocs gh-deploy`,构建产物推送到 `gh-pages` 分支，由 GitHub Pages 托管。

本地预览：

```bash
pip install mkdocs-material
mkdocs serve
```

启动后访问 `http://127.0.0.1:8000`,修改内容会实时刷新;确认无误后直接 push，部署会自动完成。

</details>
