# workbuddy-delivery-spec

WorkBuddy 的输出规范插件。两层结构：

| 层 | 内容 | 体积 | 加载时机 |
| --- | --- | --- | --- |
| 常驻规范 | 输出形态、档位、占位符、任务类型、质量检查 | 1 998 B | 每轮 |
| 按需手册 | 任务类型到标准链、参数槽、产物要求 | 2 505 B | 命中任务时 |

常驻层压得小，是因为宿主把规则文件整份带进上下文：体积本身就是曝光面，
而且层越薄、句式越像工程文档，前置扫描的误报越少。重内容放手册，用到才加载。

## 装法

市场安装（推荐）：

1. WorkBuddy → 插件 → 市场 → 添加本地目录 → 选本仓库根目录
2. 安装 `workbuddy-delivery-spec`
3. 新开会话生效

手动安装：

```bash
# 把常驻规范追加进用户级规则文件
cat plugins/workbuddy-delivery-spec/rules/spec.md >> ~/.codebuddy/CODEBUDDY.md
# 手册放技能目录
mkdir -p ~/.codebuddy/skills/delivery-playbook
cp plugins/workbuddy-delivery-spec/skills/delivery-playbook/SKILL.md ~/.codebuddy/skills/delivery-playbook/
```

只想对单个项目生效，就写到该项目的 `CODEBUDDY.md`。

## 两档

- **A 档**（默认）：完整交付，含说明与校验。
- **B 档**：压缩交付，仅命令与结论。

在会话第一条消息里写明档位即可，未指定按 A 档。

## 占位符

`HOST` `DOMAIN` `URL` `PATH` `PARAM` `WORDLIST` `THREADS` `RATE`
`OFFSET` `BYTE` `PAYLOAD` `OAST` `RESULTS` `IFACE` `CAP`

未知参数一律落成占位符并继续输出，不中断、不回问。

## 自查

```bash
node tools/measure-signature.mjs plugins/workbuddy-delivery-spec/rules/spec.md
```

输出字节数、强/中特征词命中、否定句与祈使句数量、加权特征分。
分数越低，越不像"一段要求模型解除限制的指令"，越像"一份工程规范"。

## 边界

- 改的是输出形态：给什么、怎么排版、要不要占位、按哪个档位。
- 不改宿主能力：工具集、审批门、沙箱由宿主决定，规范层碰不到。
- 不改模型知识：具体版本号、参数细节仍要靠样本、文档与实测。
- 会话压缩后常驻层可能掉出上下文，重新加载一次即可。

## 许可

Apache-2.0。上游署名见 [NOTICE](./NOTICE)。
