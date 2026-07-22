# RFrontEnd

一个父仓库，统筹多个从零开发 HTML5/CSS3/TypeScript/React 对等实现的子项目，
完全不复用任何现有实现的代码。

📖 其他语言: [日本語](README-Japan.md) / [English](README-English.md) /
[中文](README-Chinese.md) / [한국어](README-Korea.md) / [Español](README-Spain.md) /
[Français](README-France.md) / [Deutsch](README-Germany.md) / [Italiano](README-Italy.md) /
[Русский](README-Russia.md) / [العربية](README-Arabic.md) —
若要将本项目引入其他项目，只需阅读 **[PORTING.md](PORTING.md)** 这一份文件即可。

## 子项目一览

| 项目 | 角色 | 仓库 |
|---|---|---|
| RHTML | HTML5 对等的解析器/DOM 实现 | [RTHML](https://github.com/aon-co-jp/RTHML) |
| RCSS | CSS3 对等的解析器/层叠计算 | [RCSS](https://github.com/aon-co-jp/RCSS) |
| RTypeScript | TypeScript 对等实现 | [RTypeScript](https://github.com/aon-co-jp/RTypeScript) |
| RJSON | JSON 处理 | [RJSON](https://github.com/aon-co-jp/RJSON) |
| RReact | React 对等的组件模型 | [RReact](https://github.com/aon-co-jp/RReact) |

服务器端运行基础由 [RPoem](https://github.com/aon-co-jp/RPoem)(从零重新开发的
Poem)和 [RCosmo](https://github.com/aon-co-jp/RCosmo)(原 open-runo)负责。

详细的开发方针、开发顺序、架构请参阅 `CLAUDE.md`。

## 许可证

Apache-2.0 OR MIT
