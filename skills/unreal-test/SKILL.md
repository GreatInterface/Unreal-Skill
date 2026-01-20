---
name: unreal-test
description: 为Unreal Engine生成符合CQTest规范的gameplay测试代码；支持基础测试、Actor测试、Animation测试、Input测试、Network测试、Map测试等多种类型；提供完整的代码模板和测试最佳实践指导；帮助开发者快速编写高质量的游戏测试用例
---

# Unreal Engine CQTest测试代码生成

## 任务目标
- 本 Skill 用于：为Unreal Engine项目生成符合CQTest规范的gameplay自动化测试代码
- 能力包含：
  - 生成基础测试类和测试方法
  - 创建Actor、Animation、Input、Network、Map等专项测试
  - 提供完整的测试模板和Helper类
  - 指导使用Latent Actions和Command Builder
- 触发条件：用户需要为Unreal Engine项目编写或生成自动化测试代码时

## 前置准备
- CQTest模块已添加到项目Build.cs文件的PrivateDependencyModuleNames中
- 已在UE编辑器中启用Code Quality Unreal Test Plugin（可选，用于参考）
- 项目已配置好编译环境
```c#
public class TestsRuntime : ModuleRules
{
	public TestsRuntime(ReadOnlyTargetRules Target) : base(Target)
	{
		PrivateDependencyModuleNames.AddRange(
			new string[]
			{
				"CQTest",
				"CQTestEnhancedInput",
			}
		);
		

		if (Target.bBuildEditor)
		{
			PrivateDependencyModuleNames.AddRange(
				new string[] {
					"EngineSettings",
					"LevelEditor",
					"UnrealEd"
			});
		}
	}
}
```

## 操作步骤

### 1. 确定测试类型
根据测试需求选择合适的测试类型：
- **基础测试**：简单的逻辑验证和断言 → 使用 `TEST` 宏
- **结构化测试**：需要setup/teardown、共享状态的测试 → 使用 `TEST_CLASS` 宏
- **Actor测试**：测试Actor行为和生命周期 → 使用 ActorTestHelper
- **Animation测试**：测试动画播放和状态 → 使用 AnimationTestHelper
- **Input测试**：测试输入系统和输入动作 → 使用 InputTestHelper
- **Network测试**：测试网络复制和同步 → 使用 PIENetworkComponent 和 AsyncMessageTestActor
- **Map测试**：在指定关卡中测试 → 使用 MapTestSpawner

### 2. 生成测试代码
根据选择的测试类型，从 `assets/templates/` 中选择相应的模板文件：
- 基础测试：`basic-test-template.cpp` 或 `test-class-template.cpp`
- Actor测试：`actor-test-template.h` 和 `actor-test-template.cpp`
- Animation测试：`animation-test-template.h` 和 `animation-test-template.cpp`
- Input测试：`input-test-template.h` 和 `input-test-template.cpp`
- Network测试：`network-test-template.h` 和 `network-test-template.cpp`
- Map测试：`map-test-template.cpp`

根据实际需求：
1. 替换命名空间、类名、测试方法名
2. 根据测试场景调整BEFORE_EACH/AFTER_EACH逻辑
3. 添加必要的测试断言（ASSERT_THAT宏）
4. 如果需要，引入Latent Actions（FExecute、FWaitUntil等）

### 3. 使用Helper类
根据测试类型引用对应的Helper头文件：
- Actor测试：`#include "Helpers/ActorTestHelper.h"`
- Animation测试：`#include "Helpers/AnimationTestHelper.h"`
- Input测试：`#include "Helpers/InputTestHelper.h"`

Helper类提供了简化的API来创建测试环境和管理测试对象。

### 4. 添加Latent Actions（如需要）
对于需要多帧执行的测试，使用Latent Actions：
```cpp
// 使用FWaitUntil等待条件满足
AddCommand(new FWaitUntil([&]() {
    return TargetActor->GetCurrentState() == ExpectedState;
}));

// 使用TestCommandBuilder链式调用
TestCommandBuilder
    .Do([&]() { SpawnActor(); })
    .Until([&]() { return ActorReady(); })
    .Then([&]() { ASSERT_THAT(IsTrue(Actor->IsValid())); });
```

### 5. 运行测试
在Unreal Editor中运行测试：
1. 打开 Tools → Sessions Frontend
2. 在测试列表中找到对应的测试
3. 选择要运行的测试并点击 Start Tests

## 资源索引

### 参考文档
- **CQTest框架核心概念**：见 [references/cqtest-framework.md](references/cqtest-framework.md)（测试宏、断言、Setup/Teardown）
- **测试组件说明**：见 [references/test-components.md](references/test-components.md)（SpawnHelper、PIENetworkComponent等组件）
- **测试类型详解**：见 [references/test-types.md](references/test-types.md)（各类测试的使用场景和示例）
- **Latent Actions**：见 [references/latent-actions.md](references/latent-actions.md)（异步操作和命令构建器）
- **最佳实践**：见 [references/best-practices.md](references/best-practices.md)（测试命名、可维护性建议）

### 代码模板
- **基础测试模板**：见 [assets/templates/basic-test-template.cpp](assets/templates/basic-test-template.cpp) 和 [assets/templates/test-class-template.cpp](assets/templates/test-class-template.cpp)
- **Actor测试模板**：见 [assets/templates/actor-test-template.h](assets/templates/actor-test-template.h) 和 [assets/templates/actor-test-template.cpp](assets/templates/actor-test-template.cpp)
- **Animation测试模板**：见 [assets/templates/animation-test-template.h](assets/templates/animation-test-template.h) 和 [assets/templates/animation-test-template.cpp](assets/templates/animation-test-template.cpp)
- **Input测试模板**：见 [assets/templates/input-test-template.h](assets/templates/input-test-template.h) 和 [assets/templates/input-test-template.cpp](assets/templates/input-test-template.cpp)
- **Network测试模板**：见 [assets/templates/network-test-template.h](assets/templates/network-test-template.h) 和 [assets/templates/network-test-template.cpp](assets/templates/network-test-template.cpp)
- **Map测试模板**：见 [assets/templates/map-test-template.cpp](assets/templates/map-test-template.cpp)

### Helper类
- **ActorTestHelper**：见 [assets/helpers/ActorTestHelper.h](assets/helpers/ActorTestHelper.h)（Actor测试辅助工具）
- **AnimationTestHelper**：见 [assets/helpers/AnimationTestHelper.h](assets/helpers/AnimationTestHelper.h)（动画测试辅助工具）
- **InputTestHelper**：见 [assets/helpers/InputTestHelper.h](assets/helpers/InputTestHelper.h)（输入测试辅助工具）

## 注意事项
- 优先使用Latent Actions（FWaitUntil）而非固定延时（FWaitDelay），以提高测试稳定性
- 确保每个测试都是独立的，不依赖于其他测试的执行顺序
- 使用AFTER_EACH正确清理资源，避免内存泄漏
- 对于涉及网络或Blueprint的测试，需要指定 EAutomationTestFlags::EditorContext 标志
- 测试命名应清晰描述测试意图，推荐格式：`测试场景_预期行为`
- 充分利用TEST_CLASS的Setup/Teardown机制，减少重复代码
- If you have some test maps that you don't intend to ship with the game, don't put them into the /Maps folder. Projects usually mark all maps in that folder to get cooked. Put them in some other folder, and even better, put them into a folder marked in `DirectoriesToNevercook` in config

## 使用示例

### 示例1：创建基础测试
```cpp
#include "CQTest.h"

TEST(MovementTest, "Game.Character.Movement")
{
    // 测试角色移动逻辑
    const FVector ExpectedLocation = FVector(100.0f, 0.0f, 0.0f);
    ASSERT_THAT(AreEqual(ExpectedLocation, Character->GetActorLocation()));
}
```

### 示例2：使用TEST_CLASS进行Actor测试
```cpp
#include "CQTest.h"
#include "Helpers/ActorTestHelper.h"

TEST_CLASS(WeaponTest, "Game.Weapon")
{
    WeaponActor* TestWeapon = nullptr;

    BEFORE_EACH()
    {
        TestWeapon = ActorTestHelper::SpawnActor<WeaponActor>(FVector::ZeroVector);
    }

    AFTER_EACH()
    {
        if (TestWeapon)
        {
            TestWeapon->Destroy();
        }
    }

    TEST_METHOD(Fire_ShouldReduceAmmo)
    {
        const int32 InitialAmmo = TestWeapon->GetCurrentAmmo();
        TestWeapon->Fire();
        ASSERT_THAT(AreEqual(InitialAmmo - 1, TestWeapon->GetCurrentAmmo()));
    }
};
```

### 示例3：使用Latent Actions测试异步操作
```cpp
TEST_METHOD(Animation_Play_ShouldComplete)
{
    TestCommandBuilder
        .Do([&]() {
            AnimationTestHelper->PlayAnimation(TestMontage);
        })
        .Until([&]() {
            return AnimationTestHelper->IsMontagePlaying() == false;
        })
        .Then([&]() {
            ASSERT_THAT(IsTrue(AnimationCompletedSuccessfully));
        });
}
```