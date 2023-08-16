[TOC]

# 1. .NET MAUI 介绍

## 1.1. .NET MAUI 是什么

定义：全称 .Net Multi-Platform APP UI ，多平台应用 UI ，是 Microsoft 一个免费、开源的跨平台框架。

作用：使用 C# 和 XMAL 创建本机移动和桌面应用。

工作原理： .NET MAUI 将 Android、iOS、MacOS 和 Windows API 统一到单个 API 中，实现一次编译，多端运行。

![img](https://learn.microsoft.com/zh-cn/dotnet/maui/media/what-is-maui/maui-overview.png)

.NET 6 或更高版本提供一系列特定于平台的框架用于创建应用：.NET for Android、.NET for iOS、.NET for macOS 和 Windows UI 3 (WinUI 3) 库。 这些框架都有权访问同一个 .NET 基类库 (BCL) 。 此库从代码中抽象出基础平台的详细信息。 BCL 依赖于 .NET 运行 时来为代码提供执行环境。 对于 Android、iOS 和 macOS，环境由 Mono 实现，这是 .NET 运行时的实现。 在 Windows 上，.NET CoreCLR 提供执行环境。

![img](https://learn.microsoft.com/zh-cn/dotnet/maui/media/what-is-maui/architecture-diagram.png)

支持的平台：

- Android 5.0 (API 21) 或更高版本。
- iOS 11 或更高版本，使用最新版本的 Xcode。
- 使用 Mac Catalyst 的 macOS 10.15 或更高版本。
- 使用 Windows UI 库 (WinUI) 3 Windows 11和Windows 10版本 1809 或更高版本。
- 甚至支持三星的 Tizen 平台。

.NET MAUI Blazor 应用具有以下附加平台要求： 

- 需要 Android 7.0 (API 24) 或更高版本
- 需要 iOS 14 或更高版本
- 使用 Mac Catalyst 的 macOS 11 或更高版本

 .NET MAUI Blazor 应用还需要更新的平台特定的 WebView 控件。 有关详细信息，请参阅 Blazor 支持的平台。适用于 Android、iOS 和 Windows 的 .NET MAUI 应用可以在 Visual Studio 中生成。 但 是，iOS 开发需要网络 Mac。

**热重载： 即实时更新， .NET MAUI 包括对 .NET 、 XMAL 热重载的支持，使开发者可以在应用运行时修改托管源代码，而无需手动暂停或命中断点。 然后，无需重新编译即可将代码编辑应用于正在运行的应用。**此外，还将保留导航状态和数据，使开发者能够在 UI 上快速迭代，而不会失去应用中的位置。

## 1.2. .NET MAUI 和 Xamarin.Forms 的区别

.Net MAUI 是 Xamarin.Forms 的演变。

## 1.3. .NET MAUI 运行环境安装-以 Windows 为例

软件要求：Visual Studio 2022 17.3 或更高版本。

安装步骤：

- 下载 Visual Studio 2022 ，有社区版、专业版和企业版，这里下载专业版（需要破解），链接如下：
  - [下载 Visual Studio 2022 Community](https://c2rsetup.officeapps.live.com/c2r/downloadVS.aspx?sku=Community&channel=Release&Version=VS2022&source=VSLandingPage&add=Microsoft.VisualStudio.Workload.CoreEditor&add=Microsoft.VisualStudio.Workload.NetCrossPlat;includeRecommended&cid=2305)
  - [下载 Visual Studio 2022 Professional](https://c2rsetup.officeapps.live.com/c2r/downloadVS.aspx?sku=Professional&channel=Release&Version=VS2022&source=VSLandingPage&add=Microsoft.VisualStudio.Workload.CoreEditor&add=Microsoft.VisualStudio.Workload.NetCrossPlat;includeRecommended&cid=2305)
  - [下载 Visual Studio 2022 Enterprise](https://c2rsetup.officeapps.live.com/c2r/downloadVS.aspx?sku=Enterprise&channel=Release&Version=VS2022&source=VSLandingPage&add=Microsoft.VisualStudio.Workload.CoreEditor&add=Microsoft.VisualStudio.Workload.NetCrossPlat;includeRecommended&cid=2305)
- 安装 Visual Studio 或修改现有安装，并使用默认的可选安装选项安装 .NET 多 平台应用 UI 开发工作负载：

![img](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/installation/vs/vs-workloads.png)

## 1.4. .NET MAUI 项目创建

1. 启动 Visual Studio 2022。 在“开始”窗口中，单击 **创建新项目** 以创建新项目：

   ![新建解决方案。](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/first-app/vs/new-solution.png)

2. 在**创建新项目**窗口中，搜索 MAUI ，在**所有项目类型**下拉列表中选择 **MAUI**，选择 **.NET MAUI 应用**模板，然后单击**下一步**按钮：

   ![image-20230801151007382](C:\Users\JeylenBrown\AppData\Roaming\Typora\typora-user-images\image-20230801151007382.png)

3. 在**配置新项目**窗口中，为项目命名，为其选择合适的位置，然后单击**下一步**按钮：

   ![image-20230801151232476](C:\Users\JeylenBrown\AppData\Roaming\Typora\typora-user-images\image-20230801151232476.png)

4. 在**其他信息**窗口中，选择要面向的 .NET 版本，然后单击**创建**按钮：

   ![image-20230729002752329](C:\Users\JeylenBrown\AppData\Roaming\Typora\typora-user-images\image-20230729002752329.png)

5. 等待项目创建并还原其依赖项，看这些依赖项名字可以发现这些框架。解决方案 FirstDemo 的结构如下：

   ![image-20230801185717925](C:\Users\JeylenBrown\AppData\Roaming\Typora\typora-user-images\image-20230801185717925.png)

   - 项目： 一个解决方案可以有多个项目，目前只有 FirstDemo 。

     - Properties 文件夹：有 launchSettings.json 运行配置文件
     - 依赖项：各个平台的框架，能使项目部署到 Android 、 IOS 、 Windows 、 Mac 平台上
     - Platform 文件夹：开发人员能够访问特定平台的原生 API ，每个都有脚手架代码，比如 Android manifest 、定义不同权限和应用资源、应用支持的 Rtl 、有启动代码比如 MainActivity 。
     - Resourse 文件夹：包含共享的跨平台资源（Fronts、Images、Raw 等）
       - 在 Images 有个 dotnet_bot.svg 文件，如下图所示路径， svg 用于应用程序图标和前景色，编译时会自动转化为 PNG 并缩放

     ![image-20230801200916408](C:\Users\JeylenBrown\AppData\Roaming\Typora\typora-user-images\image-20230801200916408.png)

     - MauiProgram.cs ：**启动应用的代码文件，应用的跨平台入口点，用于配置和启动应用，模板启动代码指向由 App.xaml 文件定义的 App 类，最后返回 MauiApp。**

     ```c#
     namespace FirstDemo;
     public static class MauiProgram{
     	public static MauiApp CreateMauiApp(){
     		var builder = MauiApp.CreateBuilder();  // 构建一个 MauiApp 对象 builder
     		/*
     		 继续创建对象：
     			1、使用某个程序
     			2、配置字体、生命周期、依赖项服务中的服务等
     		 */
             builder
     			.UseMauiApp<App>()
     			.ConfigureFonts(fonts =>
     			{
     				fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
     				fonts.AddFont("OpenSans-Semibold.ttf", "OpenSansSemibold");
     			});
     		return builder.Build(); // 返回一个 MauiApp
         }
     }
     ```

     - App.xaml ：**包含应用范围的 XAML 资源，例如颜色、样式或模板。**
       - App.xaml.cs ：**与 XAML 文件关联的代码隐藏文件（即  C# 文件）**，每个 .xaml.cs 和 .xaml 关联。 App.xaml.cs 文件通常包含实例化 Shell 应用程序的代码。 在此项目中，它指向 `AppShell` 类。App.xaml 和 App.xaml.cs 都是继承 `App` 类的 `Application` 类。（在 .NET MAUI 应用中，XAML 主要用于定义页面的可视内容，并与代码隐藏文件协同工作。 代码隐藏文件为标记提供代码支持。 这两个文件共同构成一个新的类定义，其中包括子视图和属性初始化。 在 XAML 文件中，类和属性通过 XML 元素和属性进行引用，并在标记和代码之间建立链接）

     ```c#
     namespace FirstDemo;						// 声明解决方案
     public partial class App : Application{		// App 类继承 Application 类
     	public App(){							// 构造函数
     		InitializeComponent();				// 添加新的 XAML 文件时，代码隐藏在构造函数中包含一行，即对 InitializeComponent 方法的调用：读取 XAML 标记并初始化标记定义的所有对象
     		MainPage = new AppShell();			// 表示应用程序的主页被设置为 AppShell
     	}
     }
     ```

     - AppShell.xaml ：**定义 AppShell 类**，该类用于定义应用的可视层次结构。AppShell.xaml 和 AppShell.xaml.cs 都是继承 `Shell` 类的 `AppShell` 类。

     - MainPage.xaml ： **是应用显示的启动页，定义页面，比如用户界面 UI。**
       - MainPage.xaml.cs ：包含 XAML 的代码隐藏，如按钮单击事件的代码。

   - 双击 FirstDemo，里面实现跨平台功能。如果想针对 Samsung 的 Tizen 平台开发，将以下代码取消注释，并在提供的 GitHub 网址安装 Tizen 工具。

     ![image-20230801202009820](C:\Users\JeylenBrown\AppData\Roaming\Typora\typora-user-images\image-20230801202009820.png)

     - <PropertyGroup>
       - <!-- Display name -->  项目标题
       - <!-- App Identifier --> 标识符
       - <!-- Versions --> 其中的 <SupportedOSPlatformVersion 可以添加向前向后兼容性
     - <ItemGroup>
       - <!-- App Icon -->
       - <!-- Splash Screen --> 启动屏幕
       - <!-- Images -->
       - <!-- Custom Fonts -->
       - <!-- Raw Assets (also remove the "Resources\Raw" prefix) -->

6. 在 Visual Studio 工具栏中，使用**调试目标**下拉列表选择**Android 模拟器**，然后使用**Android 模拟器**条目：

   ![选择 .NET MAUI 的 Android 模拟器调试目标。](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/first-app/vs/android-debug-target.png)

7. 在 Visual Studio 工具栏中，按 **Android 模拟器** 按钮，Visual Studio 将开始安装默认的 Android SDK 和 Android 模拟器。

   ![Android 模拟器按钮。](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/first-app/vs/android-emulator-button.png)

8. 在 **“Android SDK - 许可协议”** 窗口中，按“ **接受”** 按钮：

   ![第一个 Android SDK 许可协议窗口。](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/first-app/vs/android-sdk-license1.png)

9. 在 **“Android SDK - 许可协议”** 窗口中，按“ **接受”** 按钮：

   ![第二个 Android SDK 许可协议窗口。](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/first-app/vs/android-sdk-license2.png)

10. 在**用户帐户控制**对话框中，按**是**按钮：

    ![Android SDK 许可证用户帐户控制对话框。](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/first-app/vs/android-sdk-license-uac.png)

11. 在**许可接受**窗口中，按**接受**按钮，等待 Visual Studio 下载 Android SDK 和 Android Emulator。

    ![Android 设备许可证窗口。](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/first-app/vs/android-device-license.png)

12. 在 Visual Studio 工具栏中，按 **Android 模拟器** 按钮，Visual Studio 将开始创建默认的 Android 模拟器。

    ![Android 模拟器按钮。](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/first-app/vs/android-emulator-button.png)

13. 在**用户帐户控制**对话框中，按**是**按钮：

    ![Android 设备管理器用户帐户控制对话框。](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/first-app/vs/android-device-manager-uac.png)

14. 在**新建设备**窗口中，按**创建**按钮，等待 Visual Studio 下载、解压缩和创建 Android 模拟器。

    ![“新建 Android 设备”窗口。](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/first-app/vs/new-android-device.png)

15. 关闭 **Android 设备管理器** 窗口：

    ![Android 设备管理器窗口。](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/first-app/vs/android-device-manager.png)

16. 在 Visual Studio 工具栏中，按 **Pixel 5 - API 30 (Android 11.0 - API 30)** 按钮生成并运行应用，Visual Studio 将启动 Android 模拟器、生成应用并将应用部署到模拟器。

    ![“Pixel 5 API 30 仿真器”按钮。](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/first-app/vs/pixel5-api30.png)

    > 注意：必须启用硬件加速才能最大程度地提高 Android 仿真器的性能。 如果不这样做，将导致仿真器运行非常缓慢。 有关详细信息，请参阅 [如何使用 Android 模拟器启用硬件加速 (Hyper-V & AEHD) ](https://learn.microsoft.com/zh-cn/dotnet/maui/android/emulator/hardware-acceleration)。

17. 在 Android 模拟器中正在运行的应用中，多次按**单击我**按钮，并观察按钮单击次数的计数递增。

    ![在 Android 模拟器中运行的应用。](https://learn.microsoft.com/zh-cn/dotnet/maui/get-started/media/first-app/vs/running-app.png)

18. 如果应用无法编译，请查看 [排查已知问题](https://learn.microsoft.com/zh-cn/dotnet/maui/troubleshooting)，其中可能提供了解决问题的方法。 如果问题与 Android 模拟器相关，请参阅 [Android 模拟器故障排除](https://learn.microsoft.com/zh-cn/dotnet/maui/android/emulator/troubleshooting)。

在编译器下拉选择 Framework 选择框架时，会自动切换开发者要部署的内容。

![image-20230801211029509](C:\Users\JeylenBrown\AppData\Roaming\Typora\typora-user-images\image-20230801211029509.png)

- 对于Android 和 Windows 可以直接部署到 Windows 设备上，如果使用 Win11 ，则可以启用适用于 Android 的 Windows 子系统，甚至不需要模拟器。如果使用 Win10 ，在工具->Android->Android 设备管理器，也有完整的设备管理器。
- 对于 IOS 部署，要么远程连接到 Mac 并通过远程模拟器进行部署和调试，要么将 IOS 设备直接插入到 Windows 后对苹果手机热重启直接部署到苹果设备。
- 对于 Mac ，需要使用 Mac 系统。

## 1.5. 在各个平台上生成应用

### 1.5.1. Android

#### 1.5.1.1. 仿真器

使用 Visual Studio，在 Android 设备不可用的情况下，可以在模拟器中轻松测试和调试适用于 Android 的 .NET MAUI 应用。 但是，如果硬件加速不可用或未启用，模拟器的运行速度将非常慢。 通过启用硬件加速并使用 *x86-64 或 x86* 虚拟设备映像，可以显著提高仿真器的性能。**因此要在仿真器上调试，首先要在 Windows 上加速 Android 模拟器，以下虚拟化技术可用于加速 Android Emulator：**

- Windows 虚拟机监控程序平台 (WHPX) 。 [Hyper-V](https://learn.microsoft.com/zh-cn/virtualization/hyper-v-on-windows/) 是 Windows 的虚拟化功能，使虚拟的计算机系统可以在物理主计算机上运行。
- Android Emulator 虚拟机监控程序驱动程序 (AEHD) 。
   备注

> 注意：Intel 硬件加速执行管理器 (HAXM) 已从模拟器 33.x.x.x 中弃用，并已由 Intel 处理器上的 AEHD 取代。 有关在模拟器 32.x.x.x 和更低版本上使用 HAXM 的信息，请参阅 developer.android.com [上的 Windows 上使用 Intel HAXM 配置 VM 加速](https://developer.android.com/studio/run/emulator-acceleration#vm-windows-haxm-intel)。

为了在 Windows 上获得最佳体验，建议使用 WHPX 来加速 Android 模拟器。 如果 WHPX 在计算机上不可用，则可以使用 AEHD。 如果满足以下条件，Android 模拟器会自动使用硬件加速：

- 硬件加速在开发计算机上可用并已启用。
- 模拟器正在运行为基于 x86-64 或 x86 的虚拟设备创建的系统映像。

> 重要：不可在另一 VM（例如由 VirtualBox、VMware 或 Docker 托管的 VM）内运行经过 VM 加速的模拟器（除非使用 WSL2）。必须[直接在系统硬件上](https://developer.android.com/studio/run/emulator-acceleration.html#extensions)运行 Android Emulator 。有关使用 Android Emulator 进行启动和调试的信息，请参阅 Android Emulator 调试。

##### 1.5.1.1.1. 使用 Hyper-V 加速

1. 首先要验证计算机硬件和软件是否与 Hyper-V 兼容，打开 cmd 并键入以下命令：`systeminfo`，如果列出的所有 Hyper-V 要求的值均为“是”，则计算机可以支持 Hyper-V。如果 Hyper-V 结果指示虚拟机监控程序当前正在运行，则 Hyper-V 已启用。

   ![检查 Hyper-V 对 .NET MAUI 的支持时的 systeminfo 输出示例。](https://learn.microsoft.com/zh-cn/dotnet/maui/android/emulator/media/hardware-acceleration/win/systeminfo-small.png)

2. 若计算机符合上述条件，在 Windows 搜索框中输入“Windows 功能”，然后在搜索结果中选择“打开或关闭 Windows 功能” 。 在“Windows 功能”对话框中，启用“Hyper-V”和“Windows 虚拟机监控程序平台” 。

   ![为 .NET MAUI 启用 Hyper-V 和 Windows 虚拟机监控程序平台。](https://learn.microsoft.com/zh-cn/dotnet/maui/android/emulator/media/hardware-acceleration/win/windows-features.png)

3. 进行这些更改后，重新启动计算机。


> 注意 1 ：在 WIN10 2018 年 10 月更新(RS5) 及更高版本中，只需启用 Hyper-V 即可，因为它将自动使用 Windows 虚拟机监控程序平台 (WHPX)。
>
> 注意 2 ：请确保在 Android 设备管理器 中创建的虚拟设备是基于 x86-64 或 x86 的系统映像。 如果使用基于 Arm 的系统映像，虚拟设备不会加速，并且运行速度缓慢。
>
> 注意 3 ： Windows 中的多个功能以隐式方式启用 Hyper-V。 有关详细信息，请参阅在 [developer.android.com 上禁用 Hyper-V 时进行双](https://developer.android.com/studio/run/emulator-acceleration#disable-hyper-v)检查。

##### 1.5.1.1.2. 使用 AEHD 加速

如果计算机不支持 Hyper-V，则应使用 AEHD 加速 Android 模拟器。 在安装和使用 AEHD 之前，首先验证计算机是否支持 AEHD ，计算机必须满足以下条件才能支持 AEHD：

- 具有虚拟化扩展的 Intel 或 AMD 处理器，必须在 BIOS 中启用。
- 64 位 WIN11、WIN10、WIN8 或 WIN7。
- 必须关闭 Hyper-V。

如果计算机满足上述条件，请使用以下步骤通过 AEHD 加速 Android 模拟器：

1. 在 Visual Studio 中，选择 “Android >> Android SDK 管理器...” 菜单项。

2. 在 “Android SDK 和工具” 窗口中，选择“ 工具 ”选项卡。

3. 在 “工具”选项卡中，展开“ 附加项”，勾选 Android Emulator 虚拟机监控程序驱动程序 (安装程序) 项的复选框，然后选择“ 应用更改” 按钮：

   ![通过 Visual Studio 中的 Android SDK 管理器安装 AEHD。](https://learn.microsoft.com/zh-cn/dotnet/maui/android/emulator/media/hardware-acceleration/win/aehd.png)

   > 注意：或者可以从 GitHub 下载并安装 AEHD。 解压缩驱动程序包后，使用管理员权限在命令行中运行 silent_install.bat 。

4. 请确保在 Android 设备管理器 中创建的虚拟设备是基于 x86-64 或 x86 的系统映像。 如果使用基于 Arm 的系统映像，虚拟设备不会加速，并且运行速度缓慢。

5. 安装后，打开 cmd 使用键入命令 `sc query aehd` 确认驱动程序正常运行

#### 1.5.1.2. 设备

虽然**Android 仿真器**是快速开发和测试应用的好方法，但最后还是需要在真实的 Android 设备上测试应用，并且对低配计算机不友好。若要在设备上运行，需要在设备上启用开发人员模式并将其连接到计算机。

##### 1.5.1.2.1. 在设备上启用开发人员模式

设备必须启用开发人员模式才能部署和测试 Android 应用。 按照以下步骤启用开发人员模式：

1. 转到“设置” 屏幕。
2. 选择“关于手机”。
3. 点击**生成编号**七次，直到**你现在是开发人员！**可见。

![Android 上的“开发人员选项”屏幕。](https://learn.microsoft.com/zh-cn/dotnet/maui/android/device/media/setup/build-version-sml.png)

##### 1.5.1.2.2. 启用 USB 调试

1. 转到“设置” 屏幕。
2. 选择“ **开发人员选项**”。
3. 打开 **USB 调试** 选项。

##### 1.5.1.2.3. 通过 USB 将设备连接到计算机

如果以前未使用过调试设备上的计算机，将提示开发者信任该计算机。还可以选中**始终允许连接此计算机**，从而避免每次连接设备时都出现此提示。

![来自计算机的 Android 信任提示以使用 USB 调试。](https://learn.microsoft.com/zh-cn/dotnet/maui/android/device/media/setup/trust-computer-for-usb-debugging.png)

如果计算机在接通电源时无法识别设备，请尝试为设备安装驱动程序。还可以尝试通过 Android SDK 管理器安装 Google USB 驱动程序：

![已选择 Google USB 设备驱动程序的 Android SKD 管理器。](https://learn.microsoft.com/zh-cn/dotnet/maui/android/device/media/setup/google-usb-driver.png)

##### 1.5.1.2.4. 启用 WiFi 调试

可以通过 WiFi 调试 Android 设备，适用于设备离计算机太远，无法物理连接。

##### 1.5.1.2.5. 通过 WiFi 连接

默认情况下，Android Debug Bridge (adb) 配置为通过 USB 与 Android 设备通信。 可以将其重新配置为使用 TCP/IP 而不是 USB。 为此，设备和计算机必须处于同一 WiFi 网络上。

- 在 Android 设备上启用无线调试：
  1. 按照在 [设备上启用开发人员模式](https://learn.microsoft.com/zh-cn/dotnet/maui/android/device/setup#enable-developer-mode-on-the-device) 部分中的步骤进行操作。
  2. 按照 [启用 USB 调试](https://learn.microsoft.com/zh-cn/dotnet/maui/android/device/setup#enable-usb-debugging) 部分中的步骤进行操作。
  3. 转到**设置**屏幕。
  4. 选择**开发人员选项**。
  5. 打开**无线调试**选项。

- 通过 USB 连接使用 adb 连接到设备：

  1. 确定 Android 设备的 IP 地址。 查找 IP 地址的一种方法是在**设置->网络和Internet->WIFi**下查找，然后点击设备连接到的 WiFi 网络，然后点击**高级**。 这会打开一个下拉列表，其中显示了有关网络连接的信息，类似于以下屏幕截图中所示的内容。在某些版本的 Android 上，不会在此处列出 IP 地址，但可以在**“设置”“关于手机>状态”>**下找到。

     ![包含 IP 地址的 Android 状态屏幕。](https://learn.microsoft.com/zh-cn/dotnet/maui/android/device/media/setup/ip-settings-sml.png)

  2. 在 Visual Studio 中，通过选择菜单选项打开 adb 命令提示符： **工具**->**Android**->**Adb 命令提示符...**。

  3. 打开cmd键入命令 `adb tcpip 555` 命令告知设备侦听端口 5555 上的 TCP/IP 连接。

  4. 断开 USB 电缆与设备的连接。

  5. 使用端口 5555 连接到设备的 IP 地址：

     ```command
     adb connect 192.168.1.28:5555
     ```

  6. 此命令完成后，Android 设备将通过 WiFi 连接到计算机。

  7. 通过 WiFi 完成调试后，可以使用以下命令 `adb usb` 将 ADB 重置回 USB 模式。

  8. 若要查看连接到计算机的设备，使用 `adb devices` 命令。

### 1.5.2. Windows

#### 1.5.2.1. 在 Windows 中启用开发人员模式。

1. 打开“开始”菜单。
2. 搜索 **开发人员设置**，将其选中。
3. 打开 **开发人员模式**。
4. 如果收到有关开发人员模式的警告消息，请阅读该消息，如果理解该警告，请选择 **是** 。

![MAUI .NET Windows 应用Windows 11中的开发人员模式设置。](https://learn.microsoft.com/zh-cn/dotnet/maui/windows/media/setup/developer-mode-win11.png)

#### 1.5.2.2. 目标 Windows

在 Visual Studio 中，将 **调试目标** 设置为 **Framework (...)** >**net6.0-windows**。

![Visual Studio 调试目标设置为使用 .NET 7 的 Windows for .NET MAUI 应用。](https://learn.microsoft.com/zh-cn/dotnet/maui/windows/media/setup/vs-target-windows-net7.png)
