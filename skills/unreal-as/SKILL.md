---
name: unreal-as
description: 面向进阶开发者的Unreal Engine AngelScript API快速查询工具；提供按场景组织的API文档和示例代码；涵盖类系统、数据结构、生命周期、输入控制、物理碰撞、AI系统、工具类和高级特性
---

# Unreal Engine AngelScript API 快速查询

## Skill 定位
本 Skill 为进阶开发者提供 Unreal Engine AngelScript API 的快速查询能力，按实际使用场景组织 API 文档和示例代码。

## 快速索引

按开发场景快速定位相关文档：

| 场景                                              | 参考文档                                                     |
| ------------------------------------------------- | ------------------------------------------------------------ |
| 创建自定义 Actor/Component、设置属性、定义函数    | [api-class-system.md](references/api-class-system.md)        |
| 使用数组、映射、结构体、枚举管理数据              | [api-data-structures.md](references/api-data-structures.md)  |
| 实现 BeginPlay、Tick、构造脚本、委托事件          | [api-lifecycle-events.md](references/api-lifecycle-events.md) |
| 绑定输入、处理角色控制、键盘事件                  | [api-input-control.md](references/api-input-control.md)      |
| 检测重叠、移动对象、物理交互                      | [api-physics-collision.md](references/api-physics-collision.md) |
| 创建 Behavior Tree 节点（Decorator/Service/Task） | [api-ai-system.md](references/api-ai-system.md)              |
| 使用数学函数、定时器、格式化字符串                | [api-utilities.md](references/api-utilities.md)              |
| 使用 Mixin 方法、组件系统、访问修饰符             | [api-advanced-features.md](references/api-advanced-features.md) |
| 使用 Gameplay Ability System (GAS) 监听属性变化   | [api-gameplay-ability-system.md](references/api-gameplay-ability-system.md) |

## 典型使用场景

### 场景 1：创建可配置的自定义 Actor
**目标**：创建一个具有可编辑属性、自动组件和蓝图可调用方法的 Actor
**操作步骤**：
1. 继承 `AActor` 创建新类
2. 使用 `UPROPERTY()` 定义可编辑属性，配合修饰符控制编辑器行为
3. 使用 `UPROPERTY(DefaultComponent)` 自动创建组件
4. 使用 `UFUNCTION(BlueprintOverride)` 覆盖生命周期函数
5. 使用 `UFUNCTION(BlueprintCallable/BlueprintEvent)` 定义蓝图可调用的方法

**参考文档**：[api-class-system.md](references/api-class-system.md)

### 场景 2：实现角色输入控制
**目标**：创建支持键盘输入、角色移动和跳跃的角色
**操作步骤**：
1. 继承 `ACharacter` 创建角色类
2. 添加 `UInputComponent` 组件
3. 在 `BeginPlay()` 中绑定输入动作和轴
4. 实现输入处理函数（必须标记为 `UFUNCTION()`）
5. 在处理函数中调用 `AddMovementInput()`、`Jump()` 等控制函数

**参考文档**：[api-input-control.md](references/api-input-control.md)

### 场景 3：检测碰撞和重叠
**目标**：检测 Actor 之间的重叠事件并执行响应逻辑
**操作步骤**：
1. 在 Actor 类中覆盖 `ActorBeginOverlap()` 和 `ActorEndOverlap()`
2. 或在组件中绑定 `OnComponentBeginOverlap` 和 `OnComponentEndOverlap` 事件
3. 在处理函数中访问 `OtherActor` 或 `OtherComponent` 参数

**参考文档**：[api-physics-collision.md](references/api-physics-collision.md)

### 场景 4：创建自定义 Behavior Tree 节点
**目标**：扩展 AI 行为树，实现自定义的 Decorator、Service 或 Task
**操作步骤**：
1. 继承 `UBTDecorator_BlueprintBase`、`UBTService_BlueprintBase` 或 `UBTTask_BlueprintBase`
2. 覆盖对应的生命周期函数：`PerformConditionCheckAI()`、`TickAI()`、`ExecuteAI()` 等
3. 使用 `FinishExecute()` 通知行为树任务完成
4. 使用 `UPROPERTY()` 暴露可编辑参数到行为树编辑器

**参考文档**：[api-ai-system.md](references/api-ai-system.md)

### 场景 5：使用定时器延迟执行
**目标**：在指定时间后执行函数，支持暂停、恢复和取消
**操作步骤**：
1. 使用 `System::SetTimer()` 创建定时器，传入对象、函数名、时间和循环标志
2. 函数必须标记为 `UFUNCTION()`
3. 使用返回的 `FTimerHandle` 进行控制（暂停、恢复、取消）
4. 使用 `System::ClearAndInvalidateTimerHandle()` 清理定时器

**参考文档**：[api-utilities.md](references/api-utilities.md)

### 场景 6：使用 GAS AttributeSet 与属性监听
**目标**：定义 AttributeSet、注册到角色，并在动画实例中监听属性变化
**操作步骤**：
1. 继承 `UAngelscriptAttributeSet` 定义属性集并提供复制回调
2. 在 `AAngelscriptGASCharacter` 的 `BeginPlay()` 中注册属性集
3. 在消费端（如 `UAnimInstance`）注册 `OnAttributeSetRegistered` 监听
4. 使用 `GetAndRegisterCallbackForAttribute()` 缓存属性并绑定 `OnAttributeChanged`

**参考文档**：[api-gameplay-ability-system.md](references/api-gameplay-ability-system.md)

## 使用指南

### 查询方式
1. **按场景查询**：根据当前开发任务，从"典型使用场景"中选择最接近的场景
2. **按类别查询**：使用"快速索引"表，根据需求类别定位文档
3. **直接搜索**：在相关文档中搜索关键词（如 `UPROPERTY`、`TArray`、`SetTimer`）

### 文档阅读顺序
每个 reference 文档按以下结构组织：
- 场景描述：说明该文档适用的开发场景
- API 概览：按子类别列出关键 API
- 核心示例：从原始示例中提取的精简代码
- 注意事项：使用时的关键约束和最佳实践

### 示例代码使用
- 示例代码可直接复制到项目中使用
- 根据实际需求调整参数和逻辑
- 注意保留必要的修饰符标记（`UPROPERTY()`、`UFUNCTION()` 等）

## 注意事项
- ue-as 不支持 `UINTERFACE()` 和 `TScriptInterface`, 如果用户要求需要使用接口（e.g. 交互系统），请告知用户是否用 `Component` 进行代替
- 所有暴露给蓝图/编辑器的成员必须使用 `UPROPERTY()` 或 `UFUNCTION()` 标记
- 修改 struct 属性需要完整重载，无法热重载
- 定时器和委托绑定的函数必须是 `UFUNCTION()`
- 使用 `default` 关键字设置组件和父类属性，等价于 C++ 构造函数
- 访问修饰符可以精细控制不同类别的访问权限
