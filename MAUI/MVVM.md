[TOC]

# 2. MVVM

## 2.1. 将应用从 .NET 6 升级到 .NET 7

1. 在 Visual Studio、Visual Studio Code或任何其他文本编辑器中打开 *Notes.csproj* 文件。

2. 查找 `Project/PropertyGroup/TargetFrameworks` 元素。 如果它与下面的代码片段匹配，则可以跳过本教程步骤，否则将其更改为以下内容：

   ```xml
   <TargetFrameworks>net7.0-android;net7.0-ios;net7.0-maccatalyst</TargetFrameworks>
   ```

3. `Project/PropertyGroup/TargetFrameworks`找到具有指定 Windows 的 `condition` 元素，并将其更改为以下内容：

   ```xml
   <TargetFrameworks Condition="$([MSBuild]::IsOSPlatform('windows'))">$(TargetFrameworks);net7.0-windows10.0.19041.0</TargetFrameworks>
   ```

4. 更改 iOS 和 Mac 所需的最低操作系统版本。 将这两 `SupportedOSPlatformVersion` 个元素替换为以下内容：

   ```xml
   <SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'ios'">11.0</SupportedOSPlatformVersion>
   <SupportedOSPlatformVersion Condition="$([MSBuild]::GetTargetPlatformIdentifier('$(TargetFramework)')) == 'maccatalyst'">13.1</SupportedOSPlatformVersion>
   ```

5. 保存项目文件。

6. 如果使用 Visual Studio 打开了项目，请关闭 Visual Studio。

7. 删除 Notes.csproj 文件所在的文件夹中的 ./bin 和 ./obj 文件夹。

## 2.2. 了解 MVVM

引入： .NET MAUI 开发者通常**在 XAML 中创建用户界面，然后在用户界面上添加代码隐藏**。 随着应用的修改以及规模的扩大，可能会出现复杂的维护问题和较大的维护成本，主要问题是 **UI 控件和业务逻辑之间的紧密耦合**。

解决：

- **将应用的业务和呈现的逻辑与其用户界面 (UI) 完全分离**，保持应用逻辑与 UI 之间的干净分离有助于解决许多开发问题，使应用更易于测试、维护和改进。
- 显著改善代码重用机会，并允许开发者和 UI 设计者在开发应用的各个部分时更轻松地进行协作。

### 2.2.1. 模式

**定义： MVVM ，即 Model-View-ViewModel 模式中有三个核心组件：视图、视图模型和模型**。 每个组件的用途不同。 下图显示三个组件之间的关系。

![演示 MVVM 建模应用程序的各个部分的关系图](https://learn.microsoft.com/zh-cn/dotnet/maui/tutorials/notes-mvvm/media/mvvm/mvvm-pattern.png)

由上图可知，视图模型将视图与模型隔离开来，并允许模型独立于视图进行演变。视图“了解”视图模型，视图模型“了解”模型，但模型不知道视图模型，而视图模型不知道视图。

使用 MVVM 的关键：

1. 了解每个组件的**责任**；
2. 了解如何将应用代码分解为正确的类以及这些类的**交互方式**。

#### 2.2.1.1. 视图（用户界面）

**负责定义用户在屏幕上看到的结构、布局、外观、显示数据。** 理想情况下，每个视图都是在 XAML 中定义的，不包含业务逻辑的代码隐藏。 但是，在某些情况下，代码隐藏可能包含在 XAML 中难以实现的 UI 逻辑，例如动画。

#### 2.2.1.2. 视图模型（后台代码）

**通过数据绑定和命令模式实现视图和视图模型之间的交互，并通过更改通知事件通知视图任何状态更改。** 视图模型提供的属性和命令定义要由 UI 提供的功能，但视图决定了如何显示该功能。

- 数据绑定： MVVM 架构中的视图和视图模型之间通过数据绑定来实现交互。当视图模型中的数据发生变化时，视图会自动更新相应的内容，从而实现视图和视图模型之间的数据同步。

- 命令模式： MVVM 架构中的视图和视图模型之间通过命令模式来实现交互。视图中的用户交互事件会被转换为命令，然后传递给视图模型进行处理。视图模型中的命令可以实现一些业务逻辑，例如数据的验证、数据的格式化等。

**还负责视图与所需的任何模型类的交互。** 视图模型与模型类之间通常存在一对多关系。

每个视图模型以一种视图可以轻松使用的形式提供来自模型的数据。 为此，视图模型有时会执行数据转换。 将此数据转换置于视图模型中是一个好主意，因为它提供视图可以绑定到的属性。 例如，视图模型可能会合并两个属性的值，以便于视图显示。

> .NET MAUI 将绑定更新封送至 UI 线程。 使用 MVVM 时，可以使用 .NET MAUI 的绑定引擎将更新 UI 线程的更新从任何线程更新数据绑定的 viewmodel 属性。

#### 2.2.1.3. 模型

模型类是封装应用数据的非可视类。 因此，可以将模型视为表示应用的域模型，该模型通常包括数据模型以及业务和验证逻辑。

## 2.3. 安装库

微软有很实用的库，提供 MVVM 模式下的源代码生成器，生成代码，安装步骤如下：

1. 用 Visual Studio 打开任意一个 .sln 解决方案。
2. 右击项目名称
3. 点击 Manage NuGet Packages
4. 搜索 communitytoolkit mvvm ，下载 8.0.0 以上版本
5. 自动生成高度优化（通过数据数据缓存）的代码

![image-20230812185236666](C:\Users\JeylenBrown\AppData\Roaming\Typora\typora-user-images\image-20230812185236666.png)

## 2.4. 其他

支持数据绑定和用户界面相应后台代码，可以管理数据流

视图：显示数据

视图模型：代码隐藏但完全耦合 