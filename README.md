# Unreal Skill

面向 Unreal Engine 的技能知识库，包含 AngelScript API 快速查询与 CQTest 自动化测试指南。

## 目录结构
- `unreal-as`：AngelScript API 快速查询与示例
- `unreal-test`：CQTest 规范、模板与最佳实践

## Skills

### Unreal AngelScript
- 入口文档：`unreal-as/SKILL.md`
- 参考页：`unreal-as/references/`

覆盖场景包括类系统、数据结构、生命周期、输入控制、物理碰撞、AI 系统、工具类、高级特性、GAS 属性监听。

### Unreal CQTest
- 入口文档：`unreal-test/SKILL.md`
- 参考页：`unreal-test/references/`
- 模板与 Helper：`unreal-test/assets/`

覆盖基础测试、Actor/Animation/Input/Network/Map 测试，以及 Latent Actions 与最佳实践。

## 使用方式
1. 进入对应 skill 的 `SKILL.md` 查看使用场景与索引
2. 按需打开 `references/` 文档定位 API 与示例
3. 使用 `assets/templates/` 和 `assets/helpers/` 生成测试代码（CQTest）

## 许可
本仓库使用 MIT License，见 `LICENSE`。
