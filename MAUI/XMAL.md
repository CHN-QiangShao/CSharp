[TOC]

# 3. XAML

## 3.1. 概述

**定义：可扩展的应用程序标记语言 (XAML) 是一种基于 XML 的语言，是编程代码的替代方法，用于实例化和初始化对象，并在父子层次结构中组织这些对象。**

XAML 比等效代码具有优点：

- XAML 通常比等效代码更简洁易读。
- XML 中固有的父子层次结构允许 XAML 以更高的视觉清晰度模拟用户界面对象的父子层次结构。
- 非常适合用于 MVVM 模式。

XAML 比等效代码具有缺点，主要与标记语言固有的限制有关：

- XAML 不能包含代码。 必须在代码文件中定义所有事件处理程序。
- XAML 不能包含用于重复处理的循环。 但是，有一些控件显示数据集合，例如 [ListView](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.listview) 和 [CollectionView](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.collectionview)。
- XAML 不能包含条件处理。 但是，数据绑定可以引用基于代码的绑定转换器，该转换器实际上允许某些条件处理。
- XAML 通常无法实例化不定义无参数构造函数的类，尽管有时可以克服此限制。
- XAML 通常不能调用方法，但有时可以克服此限制。
- 没有可视化设计器可用于在 .NET MAUI 应用中生成 XAML。 所有 XAML 都必须是手写的，但开发者可以在编辑 UI 时使用 XAML 热重载来查看 UI。

XAML 基本上是 XML，但 XAML 具有一些独特的语法功能。 其中最重要的是：

- 属性元素
- 附加属性
- 标记扩展

这些功能不是 XML 扩展。 XAML 是完全合法的 XML 。但这些 XAML 语法功能以独特的方式使用 XML。

## 3.2. XAML 入门

### 3.2.1. XAML 文件剖析

MainPage.xaml 文件结构如下：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MyMauiApp.MainPage">
    ...
</ContentPage>
```

两个 XML 命名空间 `xmlns`  声明引用 microsoft.com 上的 URL 。

- 第一个命名空间声明**定义无前缀的标记引用**，引用 .NET MAUI 中的类，例如 [ContentPage](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.contentpage)。 
- 第二个命名空间声明**定义前缀 `x`** ， 用于 XAML 本身固有且受 XAML 其他实现支持的多个元素和属性。 但是根据 URL 中嵌入的年份，这些元素和属性略有不同。 .NET MAUI 支持 [2009 XAML 规范](https://learn.microsoft.com/zh-cn/dotnet/desktop/xaml-services/xaml-2009-language-features)。

`x:Class` 指定 .NET 类名称--`MyMauiApp` 命名空间中的 `MainPage` 类。 这意味着，此 XAML 文件在命名空间中定义名为 `MainPage` 的新类，该类派生 [ContentPage](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.contentpage) () 显示属性的标记`x:Class`。`MyMauiApp` 特性 `x:Class` 只能出现在 XAML 文件的根元素中，以定义派生的 C# 类。 这是 XAML 文件中定义的唯一新类。 XAML 文件中出现的其他所有内容都只是从现有类实例化并初始化。

MainPage.xaml.cs 文件如下所示：

```c#
namespace MyMauiApp;
public partial class MainPage : ContentPage{
    public MainPage(){
        InitializeComponent();
    }
}
```

类 `MainPage` 派生自 [ContentPage](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.contentpage)，是 [分部类](https://learn.microsoft.com/zh-cn/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) 定义。

当 Visual Studio 生成项目时，源生成器会生成新的 C# 源，其中包含从`MainPage`构造函数调用的方法`InitializeComponent()`，并将其添加到编译对象。

代码执行流程：

1. 运行`MauiProgram.cs` 代码，启动应用并执行 `App` 类的构造函数；
2.  `App` 类的构造函数实例化 `AppShell `。 
3. 进行 `AppShell` 类的实例化，显示应用的第一页，即 `MainPage` 。 
4.  `MainPage` 类的构造函数调用 `InitializeComponent()`，初始化 XAML 文件中定义的所有对象，在父子关系中将它们全部连接在一起，将代码中定义的事件处理程序附加到 XAML 文件中设置的事件，并将生成的对象树设置为页面的内容。

> `AppShell` 类 使用 .NET MAUI Shell 设置要显示的应用的第一页。详细请参阅 [.NET MAUI Shell](https://learn.microsoft.com/zh-cn/dotnet/maui/fundamentals/shell/) 。

### 3.2.2. 设置页面内容

[ContentPage](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.contentpage) 应包含单个子级，可以是视图或具有子视图的布局。

以下示例显示 [ContentPage](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.contentpage) 包含 的 [Label](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.label)：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage"
             Title="Hello XAML Page">
    <Label Text="Hello, XAML!"
           VerticalOptions="Center"
           HorizontalTextAlignment="Center"
           Rotation="-15"
           FontSize="18"
           FontAttributes="Bold"
           TextColor="Blue" />
</ContentPage>
```

.NET MAUI 类 ，例如 [ContentPage](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.contentpage) 或 [Label](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.label) 作为 XML 元素显示在 XAML 文件中。 该类的属性例如 ContentPage 的 `Title`  和 [Label](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.label) 的 7 个属性。某些属性是基本数据类型，例如 `Title` 属性的类型为`string` ，但是对于更复杂的类型的属性，转换器用于分析 XAML。 这些是 .NET MAUI 中派生 `TypeConverter`类。 对于上面的示例，会自动应用多个 .NET MAUI 转换器，以将字符串值转换为其正确的类型：

- `LayoutOptionsConverter` 属性的 `VerticalOptions` 。 此转换器将结构的公共静态字段 `LayoutOptions` 的名称转换为类型的 `LayoutOptions`值。
- `ColorTypeConverter` 属性的 `TextColor` 。此转换器转换类的公共静态字段 [Colors](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.graphics.colors) 的名称或具有或没有 alpha 通道的十六进制 RGB 值。

### 3.2.3. 页面导航

运行 .NET MAUI 应用时，通常会显示 `MainPage` 。若要查看其他页面，可以在 AppShell.xaml 文件中将其设置为新的启动页，或者从 `MainPage`导航到新页面。若要实现导航，在 MainPage.xaml.cs 构造函数中，可以创建一个简单的 [Button](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.button) 并使用事件处理程序导航到 `HelloXamlPage`：

```c#
public MainPage(){
    InitializeComponent();

    Button button = new Button
    {
        Text = "Navigate!",
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };
    
    button.Clicked += async (sender, args) =>
    {
        await Navigation.PushAsync(new HelloXamlPage());
    };

    Content = button;
}
```

编译和部署此应用的新版本时，屏幕上会显示一个按钮。 按下它可导航到 `HelloXamlPage`：

![旋转的标签文本的屏幕截图。](https://learn.microsoft.com/zh-cn/dotnet/maui/xaml/fundamentals/media/get-started/helloxaml1.png)

可以使用每个平台上显示的导航栏导航回 `MainPage` 。

> 备注：此导航模型的替代方法是使用 .NET MAUI Shell。 有关详细信息，请参阅 [.NET MAUI Shell 概述](https://learn.microsoft.com/zh-cn/dotnet/maui/fundamentals/shell/)。

### 3.2.4. XAML 和代码交互

大多数 [ContentPage](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.contentpage) 派生的子级是布局，如 [StackLayout](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.stacklayout) 或 [Grid](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.grid)，布局可以包含多个子元素。 在 XAML 中，这些父子关系是使用普通 XML 层次结构建立的：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="Center" />
        <Label Text="A simple Label"
               FontSize="18"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="Center" />
    </StackLayout>
</ContentPage>
```

此 XAML 文件在语法上是完整的，并生成以下 UI：

![页面上多个控件的屏幕截图。](https://learn.microsoft.com/zh-cn/dotnet/maui/xaml/fundamentals/media/get-started/xamlpluscode1.png)

虽然可以与 [Slider](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.slider) 和 [Button](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.button) 交互，但 UI 不会更新。 应将 [Slider](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.slider) 使 [Label](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.label) 显示当前值，而 [Button](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.button) 应执行某些操作。

[Slider](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.slider) 使用 [Label](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.label) 显示值完全可以在具有数据绑定的 XAML 中实现。但是，最好先查看代码解决方案。即便如此，处理 [Button](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.button) 单击肯定需要代码。 这意味着的代码隐藏文件`XamlPlusCodePage` 必须包含的事件 [Slider](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.slider) 和 `Clicked` 的 [Button](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.button) 事件的处理程序 `ValueChanged`：

```c#
namespace XamlSamples{
    public partial class XamlPlusCodePage{
        public XamlPlusCodePage(){
            InitializeComponent();
        }

        void OnSliderValueChanged(object sender, ValueChangedEventArgs args){
            valueLabel.Text = args.NewValue.ToString("F3");
        }

        async void OnButtonClicked(object sender, EventArgs args){
            Button button = (Button)sender;
            await DisplayAlert("Clicked!", "The button labeled '" + button.Text + "' has been clicked", "OK");
        }
    }
}
```

回到 XAML 文件中， [Slider](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.slider) 和 [Button](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.button) 标记需要包含引用这些处理程序的 `ValueChanged` 和 `Clicked` 事件的属性：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="Center"
                ValueChanged="OnSliderValueChanged" />
        <Label x:Name="valueLabel"
               Text="A simple Label"
               FontSize="18"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="Center"
                Clicked="OnButtonClicked" />
    </StackLayout>
</ContentPage>
```

请注意，将处理程序分配给事件与将值分配给属性的语法相同。 此外，要使 `ValueChanged` 的 [Slider](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.slider) 事件处理程序使用 [Label](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.label) 显示当前值，处理程序需要从代码中引用该对象。 因此， [Label](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.label) 需要一个名称，该名称由 `x:Name` 属性指定。 属性 `x` 的 `x:Name` 前缀指示此属性是 XAML 固有的。 分配给 `x:Name` 属性的名称与 C# 变量名称具有相同的规则。

事件处理程序 `ValueChanged` 现在可以设置 [Label](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.label) 以显示事件参数中提供的新 [Slider](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.slider) 值：

```c#
void OnSliderValueChanged(object sender, ValueChangedEventArgs args){
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

或者，处理程序可以从 参数获取 [Slider](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.slider) 生成此事件的对象， `sender` 并从中 `Value` 获取 属性：

```c#
void OnSliderValueChanged(object sender, ValueChangedEventArgs args){
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

结果任何操作 [Slider](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.slider) 都会导致其值显示在 中 [Label](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.label)：

![页面上多个控件的屏幕截图，其中显示了 Slider 值。](https://learn.microsoft.com/zh-cn/dotnet/maui/xaml/fundamentals/media/get-started/xamlpluscode2.png)

在上面的示例中， [Button](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.button) 通过使用 按钮的 显示警报`Text`来模拟对事件的响应`Clicked`。 因此，事件处理程序可以将 参数强制转换为 `sender` ， [Button](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.button) 然后访问其属性：

```c#
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!", "The button labeled '" + button.Text + "' has been clicked", "OK");
}
```

方法 `OnButtonClicked` 定义为 `async` ，因为 [DisplayAlert](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.page.displayalert) 该方法是异步的，并且应以 `await` 运算符开头，该运算符在方法完成时返回。 由于此方法从 `sender` 参数获取[Button](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.button)触发事件，因此同一处理程序可用于多个按钮。

## 3.3. 语法

XAML 主要用于实例化和初始化对象。 但通常必须将属性设置为无法轻松表示为 XML 字符串的复杂对象，有时必须在子类上设置由一个类定义的属性。 这两个需求需要**属性元素**和**附加属性**的基本 XAML 语法功能。

### 3.3.1. 属性元素

在 .NET 多平台应用 UI (.NET MAUI) XAML 中，类的属性通常设置为 XML 属性：

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="18"
       TextColor="Aqua" />
```

但是，有一种在 XAML 中设置属性的另一种方法：

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center"
       FontAttributes="Bold"
       FontSize="18">
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

这两个指定 `TextColor` 属性的示例在功能上是等效的，并启用了一些基本术语的引入：

- [Label](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.label) 是一个对象元素，是 XML 元素的 .NET MAUI 对象。
- `Text`、 `VerticalOptions` 、 `FontAttributes` 和 `FontSize` 是对元素属性，是 XML 对象的 .NET MAUI 属性。
- 第二个示例中， `TextColor` 是属性元素，是 XML 元素的 .NET MAUI 属性。

> 备注：在属性元素中，属性的值始终定义为属性元素开始标记和结束标记之间的内容。

还可以对对象的多个属性使用属性元素语法：

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="Center">
    <Label.FontAttributes>
        Bold
    </Label.FontAttributes>
    <Label.FontSize>
        Large
    </Label.FontSize>
    <Label.TextColor>
        Aqua
    </Label.TextColor>
</Label>
```

适用情况：**当属性值过于复杂而无法表示为简单字符串时。** 在属性元素标记中，可以实例化另一个对象并设置其属性。 例如，布局 [Grid](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.grid) 具有名为 `RowDefinitions` 和 `ColumnDefinitions`的属性，它们分别属于 `ColumnDefinitionCollection` 集合类型和 `RowDefinitionCollection` 集合类型，通常使用属性元素语法来设置它们：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>
        ...
    </Grid>
</ContentPage>
```

### 3.3.2. 附加属性

在前面的示例中，需要 [Grid](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.grid) 的 `ColumnDefinitions` 和`RowDefinitions`集合的属性元素来定义行和列。这表明必须有一种技术来指示 每个子项 [Grid](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.grid) 所在的行和列。

在  [Grid](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.grid) 标记的每个子元素中，使用 `Grid.Row` 和 `Grid.Column` 属性指定该子级的行和列，默认值为 0。 还可以使用 `Grid.ColumnSpan` 和 `Grid.RowSpan`（默认值为 1）指示子级是否跨多行或多列。

以下示例演示如何在 中 [Grid](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.grid) 放置子级：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.GridDemoPage"
             Title="Grid Demo Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
            <RowDefinition Height="100" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="100" />
        </Grid.ColumnDefinitions>

        <Label Text="Autosized cell"
               TextColor="White"
               BackgroundColor="Blue" />
        <BoxView Color="Silver"
                 Grid.Column="1" />
        <BoxView Color="Teal"
                 Grid.Row="1" />
        <Label Text="Leftover space"
               Grid.Row="1" Grid.Column="1"
               TextColor="Purple"
               BackgroundColor="Aqua"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />
        <Label Text="Span two rows (or more if you want)"
               Grid.Column="2" Grid.RowSpan="2"
               TextColor="Yellow"
               BackgroundColor="Blue"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />
        <Label Text="Span two columns"
               Grid.Row="2" Grid.ColumnSpan="2"
               TextColor="Blue"
               BackgroundColor="Yellow"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />
        <Label Text="Fixed 100x100"
               Grid.Row="2" Grid.Column="2"
               TextColor="Aqua"
               BackgroundColor="Red"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />

    </Grid>
</ContentPage>
```

此 XAML 生成以下布局：

![.NET MAUI 网格布局。](https://learn.microsoft.com/zh-cn/dotnet/maui/xaml/fundamentals/media/essential-syntax/grid.png)

`Row` 、 `Column` 、 `RowSpan` 和 `ColumnSpan` 属性似乎是 [Grid](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.grid) 类的属性，但 [Grid](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.grid) 类不定义任何名为`Row` 、 `Column` 、 `RowSpan` 和 `ColumnSpan` 的内容。相反， [Grid](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.grid) 类定义四个名为 `RowProperty`、 `ColumnProperty`、 `RowSpanProperty`和 `ColumnSpanProperty`的可绑定属性，**它们是称为附加属性的特殊可绑定属性类型。 它们由 Grid 类定义，但在 Grid 的子级上设置。**

> 备注：当希望在代码中使用这些附加属性时， [Grid](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.grid) 类提供名为 `SetRow` 、 `SetColumn` 、 `GetColumn` 、 `SetRowSpan` 、 `GetRowSpan` 、 `GetColumnSpan` 和 `SetColumnSpan` 的 `GetRow` 静态方法。

**附加属性：在 XAML 中可识别为包含类，并且由句点分隔的属性名称的属性。** 因为它们是由一个类 (在本例中定义的 Grid) 但附加到其他对象 (在本例中是 Grid 的子级)。在布局期间， [Grid](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.grid) 可以询问这些附加属性的值，以了解每个子级的位置。

### 3.3.3. 内容属性

在前面的示例中， [Grid](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.grid) 对象设置为 `Content` 的 [ContentPage](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.contentpage) 属性。但是， `Content` 属性未在该 XAML 中引用，但可以是：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <ContentPage.Content>
        <Grid>
            ...
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

`Content` XAML 中不需要属性，因为定义用于 .NET MAUI XAML 的元素允许有一个属性指定为 `ContentProperty` 类上的属性：

```csharp
[ContentProperty("Content")]
public class ContentPage : TemplatedPage{
    ...
}
```

指定为 `ContentProperty` 类的任何属性都意味着不需要属性的属性元素标记。 因此，上面的示例指定将起始标记和结束 [ContentPage](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.contentpage) 标记之间出现的任何 XAML 内容分配给属性 `Content` 。

许多类还具有 `ContentProperty` 属性定义。 例如， [Label](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.maui.controls.label)  的内容属性为 `Text`。

### 3.3.4. 平台差异

.NET MAUI 应用可以基于每个平台自定义 UI 外观。 这可以在 XAML 中使用 `OnPlatform`  和 `On` 类来实现：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0,20,0,0" />
            <On Platform="Android" Value="10,20,20,10" />
        </OnPlatform>
    </ContentPage.Padding>
    ...
</ContentPage>
```

`OnPlatform` 是泛型类，因此需要指定泛型类型参数，在本例中为 `Thickness`，即属性的类型 `Padding` 。 这是使用 XAML 属性实现的 `x:TypeArguments` 。 类 `OnPlatform` 定义一个 `Default` 属性，该属性可设置为将应用于所有平台的值：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness" Default="20">
            <On Platform="iOS" Value="0,20,0,0" />
            <On Platform="Android" Value="10,20,20,10" />
        </OnPlatform>
    </ContentPage.Padding>
    ...
</ContentPage>
```

在此示例中， `Padding` 属性在 iOS 和 Android 上设置为不同的值，其他平台设置为默认值。

类 `OnPlatform` 还定义属性 `Platforms` ，该属性是 `On` 的 `IList` 对象 。每个 `On` 对象都可以设置 `Platform` 和 `Value` 属性，以定义 `Thickness` 特定平台的值。此外， `On` 的 `Platforms` 属性类型为  `IList<string>`，因此，如果值相同，则可以包含多个平台：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="...">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness" Default="20">
            <On Platform="iOS, Android" Value="10,20,20,10" />
        </OnPlatform>
    </ContentPage.Padding>
    ...
</ContentPage>
```

这是在 XAML 中设置依赖于 `Padding` 平台的属性的标准方法。

>  备注：如果 `On` 对象的属性 `Value` 不能由单个字符串表示，则可以为其定义属性元素。有关详细信息，请参阅 [基于平台自定义 UI 外观](https://learn.microsoft.com/zh-cn/dotnet/maui/platform-integration/customize-ui-appearance#customize-ui-appearance-based-on-the-platform)。

## 3.4. 标记扩展

定义：由 `IMarkupExtension` 的类实现。还可以编写自己的自定义标记扩展。

作用：

- 使属性能够设置为从其他源**间接引用**的对象或值。

- 对于**共享**对象和引用整个应用中使用的常量特别重要，在数据绑定中找到了最大的实用性。

适用情况：通常，使用 XAML 将对象的属性设置为显式值，例如字符串、数字、枚举成员或转换为后台值的字符串。**但是，有时属性必须引用在其他地方定义的值，或者可能需要在运行时通过代码进行少量处理。**

在许多情况下，XAML 标记扩展在 XAML 文件中可以立即识别，因为它们显示为大括号 { 和 } 分隔的属性值，但有时标记扩展也会在标记中显示为常规元素。

> 重要：标记扩展可以具有属性，但它们不像 XML 属性那样设置。在标记扩展中，属性设置用逗号分隔，大括号内不显示引号。

### 3.4.1. 共享资源

某些 XAML 页面包含多个属性设置为相同值的视图。例如，这些 [Button](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button) 对象的许多属性设置是相同的：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">
    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="Center"
                VerticalOptions="Center"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />
        <Button Text="Do that!"
                HorizontalOptions="Center"
                VerticalOptions="Center"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />
        <Button Text="Do the other thing!"
                HorizontalOptions="Center"
                VerticalOptions="Center"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />
    </StackLayout>
</ContentPage>
```

如果需要更改其中一个属性，开发者可能希望只进行更改一次，而不是三次。**如果这是代码，则可能会使用常量和静态只读对象来帮助保持这些值的一致性且易于修改。在 XAML 中，一种流行的解决方案是将此类值或对象存储在资源字典中。**[类](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.visualelement)定义一个名为资源字典类型的`Resources`的属性，该属性是具有 `string` 类型的键和 `object` 类型的值的[字典](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.resourcedictionary)。可以将对象放入此字典中，然后从标记引用它们。

若要在页面上使用资源字典，请在页面顶部包含一对 `Resources` 属性元素标记，并在这些标记中添加资源。可以将各种类型的对象和值添加到资源字典中。这些类型必须是可实例化的。例如，它们不能是抽象类。这些类型还必须具有公共无参数构造函数。每个项目都需要一个使用 `x:Key` 属性指定的字典键：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">
    <ContentPage.Resources>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />
        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center" />
    </ContentPage.Resources>
    ...
</ContentPage>
```

在此示例中，这两个资源是结构类型 `LayoutOptions` 的值，每个资源都有一个唯一键和一个或两个属性集。在代码和标记中，使用 `LayoutOptions` 的静态字段更为常见，但在这里设置属性更方便。

> 注意：可选的资源[字典](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.resourcedictionary)标签可以作为 `Resources` 标签的子级包含在内。

然后通过使用 [`StaticResource`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.xaml.staticresourceextension) XAML 标记扩展设置其 `HorizontalOptions` 和 `VerticalOptions` 属性，按钮对象可以使用这些资源

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

[`StaticResource`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.xaml.staticresourceextension) 标记扩展始终用大括号分隔，并包含字典的键。 [`StaticResource`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.xaml.staticresourceextension)  与 .NET MAUI 也支持的 [`DynamicResource`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.xaml.dynamicresourceextension) 区分开来。[`DynamicResource`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.xaml.dynamicresourceextension) 用于运行时可能更改的值关联的字典键，而 [`StaticResource`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.xaml.staticresourceextension) 仅在构造页面上的元素时访问字典中的元素一次。每当 XAML 分析器遇到 [`StaticResource`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.xaml.staticresourceextension) 标记扩展时，它都会搜索可视化树，并使用它遇到的第一个包含该键的资源[字典](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.resourcedictionary)。

有必要在字典中存储 `BorderWidth`、`Rotation` 和 `FontSize` 属性的双精度值。XAML 方便地为常见数据类型（如 `x:Double` 和 `x:Int32`）定义标记:

```xaml
<ContentPage.Resources>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />
        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center" />
        <x:Double x:Key="borderWidth">3</x:Double>
        <x:Double x:Key="rotationAngle">-15</x:Double>
        <x:Double x:Key="fontSize">24</x:Double>        
</ContentPage.Resources>
```

可以使用与 `LayoutOptions` 值相同的方式引用这三个附加资源：

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="{StaticResource fontSize}" />
```

对于 [Color](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.graphics.color) 类型的资源，可以使用直接分配这些类型的属性时使用的相同字符串表示形式。创建资源时，将调用 .NET MAUI 中包含的类型转换器。还可以使用资源字典中的 `OnPlatform` 类为平台定义不同的值。下面的示例使用此类来设置不同的文本颜色：

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
</OnPlatform>
```

`OnPlatform` 资源获取 `x:Key` 属性，因为它是字典中的对象；以及 `x:TypeArguments` 属性，因为它是泛型类。初始化对象时，`iOS` 和 `Android` 属性将转换为[颜色](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.graphics.color)值。

以下示例显示访问六个共享值的三个按钮：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">
    <ContentPage.Resources>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />
        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center" />
        <x:Double x:Key="borderWidth">3</x:Double>
        <x:Double x:Key="rotationAngle">-15</x:Double>
        <x:Double x:Key="fontSize">24</x:Double>    
        <OnPlatform x:Key="textColor"
                    x:TypeArguments="Color">
            <On Platform="iOS" Value="Red" />
            <On Platform="Android" Value="Aqua" />
            <On Platform="WinUI" Value="#80FF80" />
        </OnPlatform>
    </ContentPage.Resources>

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />
        <Button Text="Do that!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />
        <Button Text="Do the other thing!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />
    </StackLayout>
</ContentPage>
```

以下屏幕截图验证样式的一致性：

![Screenshot of styled controls.](https://learn.microsoft.com/en-us/dotnet/maui/xaml/fundamentals/media/markup-extensions/sharedresources.png)

尽管通常在页面顶部定义 `Resources` 集合，但可以在页面上的其他元素上拥有`Resources`集合。例如，以下示例显示添加到[堆栈布局](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.stacklayout)的资源:

XAML

```xaml
<StackLayout>
    <StackLayout.Resources>
        <Color x:Key="textColor">Blue</Color>
    </StackLayout.Resources>
    ...
</StackLayout>
```

存储在资源字典中的最常见对象类型之一是 .NET MAUI [样式](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.style)，它定义属性设置的集合。有关样式的详细信息，请参阅[使用 XAML 设置应用样式](https://learn.microsoft.com/en-us/dotnet/maui/user-interface/styles/xaml).

> 注意：资源字典的目的是共享对象。因此，将[标签](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label)或[按钮](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button)等控件放在资源字典中是没有意义的。无法共享可视元素，因为同一实例不能在页面上出现两次。

### 3.4.2. x：静态标记扩展

除了 [`StaticResource`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.xaml.staticresourceextension) 标记扩展之外，还有一个 `x:Static` 标记扩展。但是，当 [`StaticResource`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.xaml.staticresourceextension) 从资源字典返回对象时，`x:Static` 访问公共静态字段、公共静态属性、公共常量字段或枚举成员。

> 注意：定义资源字典的 XAML 实现支持 [`StaticResource`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.xaml.staticresourceextension) 标记扩展，而 `x:Static` 是 XAML 的固有部分，如 `x` 前缀所示。

下面的示例演示 `x:Static` 如何显式引用静态字段和枚举成员：

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Colors.Aqua}" />
```

`x:Static` 标记扩展的主要用途是从代码中引用静态字段或属性。例如，下面是一个 `AppConstants` 类，其中包含一些静态字段，开发者可能希望在整个应用中的多个页面上使用这些字段：

```C#
namespace XamlSamples{
    static class AppConstants{
        public static readonly Color BackgroundColor = Colors.Aqua;
        public static readonly Color ForegroundColor = Colors.Brown;
    }
}
```

若要在 XAML 文件中引用此类的静态字段，需要使用 XML 命名空间声明来指示此文件所在的位置。每个附加的 XML 命名空间声明都会定义一个新前缀。若要访问根应用命名空间的本地类例如 `AppConstants` ，可以使用前缀 `local`。命名空间声明必须指示 CLR（公共语言运行时）命名空间名称，也称为 .NET 命名空间名称，这是出现在 C# `namespace`定义或 `using` 指令中的名称：

```C#
xmlns:local="clr-namespace:XamlSamples"
```

还可以为 .NET 命名空间定义 XML 命名空间声明。例如，下面是标准 .NET `System`命名空间的 `sys` 前缀，该命名空间位于 `netstandard` 程序集中。由于这是另一个程序集，因此还必须指定程序集名称，在本例中为 `netstandard`:

```C#
xmlns:sys="clr-namespace:System;assembly=netstandard"
```

> 注意：关键字 `clr-namespace` 后跟冒号，然后是 .NET 命名空间名称，后跟分号、关键字程序集、等号和`assembly`名称。

然后，可以在声明 XML 命名空间后使用静态字段：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             x:Class="XamlSamples.StaticConstantsPage"
             Title="Static Constants Page"
             Padding="5,25,5,0">
    <StackLayout>
       <Label Text="Hello, XAML!"
              TextColor="{x:Static local:AppConstants.BackgroundColor}"
              BackgroundColor="{x:Static local:AppConstants.ForegroundColor}"
              FontAttributes="Bold"
              FontSize="30"
              HorizontalOptions="Center" />
      <BoxView WidthRequest="{x:Static sys:Math.PI}"
               HeightRequest="{x:Static sys:Math.E}"
               Color="{x:Static local:AppConstants.ForegroundColor}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="100" />
    </StackLayout>
</ContentPage>
```

在此示例中，[BoxView](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.boxview) 维度设置为 `Math.PI` 和 `Math.E`，但按系数 100 缩放：

![Screenshot of controls using the x:Static markup extension.](https://learn.microsoft.com/en-us/dotnet/maui/xaml/fundamentals/media/markup-extensions/staticconstants.png)



### 3.4.3. 其他标记扩展

不常用且不可缺少的其他标记扩展：

- 如果属性默认具有非 `null` 值，但需要将其设置为 `null`，请将其设置为 `{x:Null}` 标记扩展。
- 如果属性的类型为 Type， `Type` 则可以使用标记扩展 `{x:Type someClass}` 将其分配给 `Type` 对象。
- 可以使用 `x:Array` 标记扩展在 XAML 中定义数组。此标记扩展具有一个名为 `Type` 的必需属性，该属性指示数组中元素的类型。

有关 XAML 标记扩展的详细信息，请参阅[使用 XAML 标记扩展](https://learn.microsoft.com/en-us/dotnet/maui/xaml/markup-extensions/consume).

## 3.5. 在各个平台上生成应用

.NET MAUI 数据绑定允许链接两个对象的属性，以便一个对象的更改会导致另一个对象的更改。这是一个非常有价值的工具，虽然数据绑定可以完全在代码中定义，但 XAML 提供了快捷方式和便利性。

### 3.5.1. 数据绑定

数据绑定连接两个对象的属性--**源**和**目标**。在代码中，需要执行两个步骤：

1. 目标对象的 `BindingContext` 属性必须设置为源对象；
2. 必须在目标对象上调用 `SetBinding` 方法，通常与 `Binding` 类结合使用，才能将该对象的属性绑定到源对象的属性。

目标属性必须是可绑定属性，这意味着目标对象必须派生自 [BindableObject](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.bindableobject)。例如 Label 的属性 `Text` 与可绑定属性`TextProperty`相关联。

在 XAML 中，还必须执行代码中所需的相同两个步骤，只是绑定标记扩展取代 `SetBinding` 调用和`Binding`类。但是在 XAML 中定义数据绑定时，有多种方法可以设置目标对象的 `BindingContext` 。有时它是从代码隐藏文件设置的，有时使用 [`StaticResource`](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.xaml.staticresourceextension) 或 `x:Static` 标记扩展，有时作为 `BindingContext` 属性元素标记的内容。

### 3.5.2. 视图到视图绑定

可以定义数据绑定以链接同一页上两个视图的属性。在这种情况下，可以使用 `x:Reference` 标记扩展设置目标对象的 `BindingContext`。

下面的示例包含一个滑块和两个[标签](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label)视图，其中一个视图由滑块值旋转，另一个显示[滑块](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.slider)值：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderBindingsPage"
             Title="Slider Bindings Page">
    <StackLayout>
        <Label Text="ROTATION"
               BindingContext="{x:Reference slider}"
               Rotation="{Binding Path=Value}"
               FontAttributes="Bold"
               FontSize="18"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="Center" />
        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='The angle is {0:F0} degrees'}"
               FontAttributes="Bold"
               FontSize="18"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </StackLayout>
</ContentPage>
```

[滑块](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.slider)包含一个 `x:Name` 属性，该属性由两个[标签](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label)视图使用 `x:Reference` 标记扩展引用。在本例的 `slider` ， `x:Reference` 绑定扩展定义一个名为 `Name` 的属性，以设置为引用元素的名称。但是，定义 `x:Reference` 标记扩展的 `ReferenceExtension` 类还定义了 `Name` 的 `ContentProperty` 属性，这意味着它不是必需显式的。

`Binding`标记扩展本身可以有多个属性，就像 `BindingBase` 和 `Binding` 类一样。`Binding`的 `ContentProperty` 是 `Path`，但如果路径是`Binding`标记扩展中的第一项，则可以省略标记扩展 **Path=** 部分。

第二个 `Binding` 标记扩展设置 `StringFormat` 属性。在 .NET MAUI 中，绑定不执行任何隐式类型转换，如果需要将非字符串对象显示为字符串，则必须提供类型转换器或使用 `StringFormat`.

> 重要：格式字符串必须放在单引号中。

### 3.5.3. 绑定模式

单个视图可以对其多个属性具有数据绑定。但是，每个视图只能有一个 `BindingContext`，因此该视图上的多个数据绑定必须都引用同一对象的属性。

此问题和其他问题的解决方案涉及 `Mode` 属性，该属性设置为 `BindingMode` 枚举的成员：

- `Default`
- `OneWay` — 值从源传输到目标
- `OneWayToSource` — 值从目标传输到源
- `TwoWay` — 值在源和目标之间双向传输
- `OneTime` — 数据从源传输到目标，但仅在 `BindingContext` 更改时

下面的示例演示 `OneWayToSource` 和 `TwoWay` 绑定模式的一种常见用法：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SliderTransformsPage"
             Padding="5"
             Title="Slider Transforms Page">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <!-- Scaled and rotated Label -->
        <Label x:Name="label"
               Text="TEXT"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <!-- Slider and identifying Label for Scale -->
        <Slider x:Name="scaleSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="1" Grid.Column="0"
                Maximum="10"
                Value="{Binding Scale, Mode=TwoWay}" />
        <Label BindingContext="{x:Reference scaleSlider}"
               Text="{Binding Value, StringFormat='Scale = {0:F1}'}"
               Grid.Row="1" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for Rotation -->
        <Slider x:Name="rotationSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="2" Grid.Column="0"
                Maximum="360"
                Value="{Binding Rotation, Mode=OneWayToSource}" />
        <Label BindingContext="{x:Reference rotationSlider}"
               Text="{Binding Value, StringFormat='Rotation = {0:F0}'}"
               Grid.Row="2" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationX -->
        <Slider x:Name="rotationXSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="3" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationX, Mode=OneWayToSource}" />
        <Label BindingContext="{x:Reference rotationXSlider}"
               Text="{Binding Value, StringFormat='RotationX = {0:F0}'}"
               Grid.Row="3" Grid.Column="1"
               VerticalTextAlignment="Center" />

        <!-- Slider and identifying Label for RotationY -->
        <Slider x:Name="rotationYSlider"
                BindingContext="{x:Reference label}"
                Grid.Row="4" Grid.Column="0"
                Maximum="360"
                Value="{Binding RotationY, Mode=OneWayToSource}" />
        <Label BindingContext="{x:Reference rotationYSlider}"
               Text="{Binding Value, StringFormat='RotationY = {0:F0}'}"
               Grid.Row="4" Grid.Column="1"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentPage>
```

在此示例中，四个[滑块](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.slider)视图用于控制[标签](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label)的 scale 、 Rotate 、 RotateX 、 RotateY 属性。起初，[Label](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label) 的这四个属性似乎是数据绑定目标，因为每个属性都由[滑块](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.slider)设置。但是， [Label](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label)  的 `BindingContext` 只能是一个对象，并且有四个不同的滑块。因此，四个滑块中每个`BindingContext`都设置为[标签](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label)，数据绑定在滑块的`Value`”属性上设置。通过使用 `OneWayToSource` 和 `TwoWay` 模式，这些`Value`属性可以设置源属性，这些属性是[标签](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label)的 Scale 、 Rotate 、 RotateX 和 RotateY 。

三个滑块视图上的绑定是 `OneWayToSource`，这意味着[滑块](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.slider)值会导致其 `BindingContext`（即名为 `label` 的[标签](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label)）的属性发生更改。这三个[滑块](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.slider)视图会导致[标签](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label)的“旋转`RotateX`”和`RotateY`属性发生更改`Rotate`:

![Reverse bindings.](https://learn.microsoft.com/en-us/dotnet/maui/xaml/fundamentals/media/data-binding-basics/slidertransforms.png)

但是，`Scale` 属性的绑定是`TwoWay`的。这是因为 `Scale` 属性的默认值为 1，并且使用 `TwoWay` 绑定会导致滑块初始值设置为 1 而不是 0。如果该绑定是 `OneWayToSource`，则 `Scale` 属性最初将从[滑块](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.slider)默认值设置为 0。[标签](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label)将不可见

> 注意：[类](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.visualelement)还具有 `ScaleX` 和 `ScaleY` 属性，这些属性分别在 x 轴和 y 轴上缩放。

### 3.5.4. 绑定和集合

[ListView](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.listview) 定义 `IEnumerable` 类型的 `ItemsSource` 属性，并显示该集合中的项。这些项可以是任何类型的对象。默认情况下，[ListView](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.listview) 使用每个项的 `ToString` 方法来显示该项。有时这正是您想要的，但在许多情况下，`ToString` 仅返回对象的完全限定类名。

但是，通过使用模板，可以按所需的任何方式显示 [ListView](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.listview) 集合中的项，该模板涉及派生 [Cell](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.cell) 的类。将为 [ListView](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.listview) 中的每个项克隆模板，并在模板上设置的数据绑定将传输到各个克隆。可以使用 [ViewCell](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.viewcell) 类为项创建自定义单元格。

在 `NamedColor` 类的帮助下，[ListView](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.listview) 可以显示一系列 .NET MAUI 中可用的每种命名颜色的列表：

```csharp
using System.Reflection;
using System.Text;

namespace XamlSamples{
    public class NamedColor{
        public string Name { get; private set; }
        public string FriendlyName { get; private set; }
        public Color Color { get; private set; }

        // Expose the Color fields as properties
        public float Red => Color.Red;
        public float Green => Color.Green;
        public float Blue => Color.Blue;

        public static IEnumerable<NamedColor> All { get; private set; }

        static NamedColor(){
            List<NamedColor> all = new List<NamedColor>();
            StringBuilder stringBuilder = new StringBuilder();

            // Loop through the public static fields of the Color structure.
            foreach (FieldInfo fieldInfo in typeof(Colors).GetRuntimeFields()){
                if (fieldInfo.IsPublic &&
                    fieldInfo.IsStatic &&
                    fieldInfo.FieldType == typeof(Color)){
                    // Convert the name to a friendly name.
                    string name = fieldInfo.Name;
                    stringBuilder.Clear();
                    int index = 0;

                    foreach (char ch in name){
                        if (index != 0 && Char.IsUpper(ch)){
                            stringBuilder.Append(' ');
                        }
                        stringBuilder.Append(ch);
                        index++;
                    }

                    // Instantiate a NamedColor object.
                    NamedColor namedColor = new NamedColor
                    {
                        Name = name,
                        FriendlyName = stringBuilder.ToString(),
                        Color = (Color)fieldInfo.GetValue(null)
                    };

                    // Add it to the collection.
                    all.Add(namedColor);
                }
            }
            all.TrimExcess();
            All = all;
        }
    }
}
```

每个 `NamedColor` 对象都具有`string`类型的 `Name` 和 `FriendlyName` 属性、Color 类型的 `Color` 属性以及`Red`、`Green`和`Blue`属性。此外，`NamedColor` 静态构造函数创建一个 `IEnumerable<NamedColor>` 集合，该集合包含与 [Colors](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.graphics.colors) 类中 [Color](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.graphics.color) 类型的字段对应的 `NamedColor` 对象，并将其分配给其公共静态 `All` 属性。

将静态 `NamedColor.All` 属性设置为 [ListView](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.listview) 的 `ItemsSource` 可以使用 `x:Static` 标记扩展来实现：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples;assembly=XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">
    <ListView ItemsSource="{x:Static local:NamedColor.All}" />
</ContentPage>
```

结果确定这些项的类型为 `XamlSamples.NamedColor`:

![Binding to a collection.](https://learn.microsoft.com/en-us/dotnet/maui/xaml/fundamentals/media/data-binding-basics/listview1.png)

若要为项定义模板，应将 `ItemTemplate` 设置为 DataTemplate 来引用 [ViewCell](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.viewcell) 。[ViewCell](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.viewcell) 应定义一个或多个视图的布局以显示每个项目：

```xaml
<ListView ItemsSource="{x:Static local:NamedColor.All}">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Label Text="{Binding FriendlyName}" />
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

> 注意：单元格和子项的绑定源是 `ListView.ItemsSource` 集合。

在此示例中，[Label](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label) 元素设置为 [ViewCell](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.viewcell) 的 [View](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.view) 属性。 不需要 `ViewCell.View` 标记因为 [View](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.view) 属性是 [ViewCell](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.viewcell) 的内容属性。此 XAML 显示每个 `NamedColor` 对象的 `FriendlyName` 属性：

![Binding to a collection with a DataTemplate.](https://learn.microsoft.com/en-us/dotnet/maui/xaml/fundamentals/media/data-binding-basics/listview2.png)

可以展开项模板以显示更多信息和实际颜色：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.ListViewDemoPage"
             Title="ListView Demo Page">
    <ContentPage.Resources>
        <x:Double x:Key="boxSize">50</x:Double>
        <x:Int32 x:Key="rowHeight">60</x:Int32>
        <local:FloatToIntConverter x:Key="intConverter" />
    </ContentPage.Resources>

    <ListView ItemsSource="{x:Static local:NamedColor.All}"
              RowHeight="{StaticResource rowHeight}">
        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <StackLayout Padding="5, 5, 0, 5"
                                 Orientation="Horizontal"
                                 Spacing="15">
                        <BoxView WidthRequest="{StaticResource boxSize}"
                                 HeightRequest="{StaticResource boxSize}"
                                 Color="{Binding Color}" />
                        <StackLayout Padding="5, 0, 0, 0"
                                     VerticalOptions="Center">
                            <Label Text="{Binding FriendlyName}"
                                   FontAttributes="Bold"
                                   FontSize="14" />
                            <StackLayout Orientation="Horizontal"
                                         Spacing="0">
                                <Label Text="{Binding Red,
                                                      Converter={StaticResource intConverter},
                                                      ConverterParameter=255,
                                                      StringFormat='R={0:X2}'}" />                                
                                <Label Text="{Binding Green,
                                                      Converter={StaticResource intConverter},
                                                      ConverterParameter=255,
                                                      StringFormat=', G={0:X2}'}" />                                
                                <Label Text="{Binding Blue,
                                                      Converter={StaticResource intConverter},
                                                      ConverterParameter=255,
                                                      StringFormat=', B={0:X2}'}" />
                            </StackLayout>
                        </StackLayout>
                    </StackLayout>
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>
```

### 3.5.5. 绑定值转换器

前面的 XAML 示例显示每个 `NamedColor` 的各个 `Red` 、 `Green` 和 `Blue` 属性。这些属性的类型为 `float`，范围为 0 到 1。如果要显示十六进制值，则不能简单地将 `StringFormat` 与“X2”格式规范一起使用。这仅适用于整数，此外，`float`值需要乘以 255。

此问题可以通过值转换器（也称为绑定转换器）来解决。这是一个实现 [IValueConverter](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.ivalueconverter) 接口的类，这意味着它有两个名为 `Convert` 和 `ConvertBack` 的方法。当值从源传输到目标时，将调用 `Convert` 方法。在 `OneWayToSource` 或 `TwoWay` 绑定中，为从目标到源的传输调用 `ConvertBack` 方法：

```C#
using System.Globalization;
namespace XamlSamples{
    public class FloatToIntConverter : IValueConverter{
        public object Convert(object value, Type targetType, object parameter, CultureInfo culture){
            float multiplier;
            if (!float.TryParse(parameter as string, out multiplier))
                multiplier = 1;
            return (int)Math.Round(multiplier * (float)value);
        }

        public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture){
            float divider;
            if (!float.TryParse(parameter as string, out divider))
                divider = 1;
            return ((float)(int)value) / divider;
        }
    }
}
```

> 注意：`ConvertBack`在此示例中不起作用，因为绑定只是从源到目标的一种方式。

绑定引用具有 `Converter` 属性的绑定转换器。绑定转换器还可以接受使用 `ConverterParameter` 属性指定的参数。对于某些多功能性，这就是指定乘数的方式。绑定转换器检查转换器参数中是否有有效的浮  `float `值。

转换器在页面的资源字典中实例化，因此可以在多个绑定之间共享：

```xaml
<local:FloatToIntConverter x:Key="intConverter" />
```

三个数据绑定引用此单个实例：

```xaml
<Label Text="{Binding Red,
                      Converter={StaticResource intConverter},
                      ConverterParameter=255,
                      StringFormat='R={0:X2}'}" />
```

项模板 dsplay 颜色、其友好名称和 RGB 值：

![Binding to a collection with a DataTemplate and a converter.](https://learn.microsoft.com/en-us/dotnet/maui/xaml/fundamentals/media/data-binding-basics/listview3.png)

[ListView](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.listview) 可以处理基础数据中动态发生的更改，但前提是您执行某些步骤。如果分配给 [ListView](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.listview) 的 `ItemsSource` 属性的项的集合在运行时发生更改，请对这些项使用 [ObservableCollection](https://learn.microsoft.com/en-us/dotnet/api/system.collections.objectmodel.observablecollection-1) 类。[ObservableCollection](https://learn.microsoft.com/en-us/dotnet/api/system.collections.objectmodel.observablecollection-1) 实现 `INotifyCollectionChanged` 接口，[ListView](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.listview) 将为 `CollectionChanged` 事件安装处理程序。

如果项本身的属性在运行时发生更改，则集合中的项应实现 `INotifyPropertyChanged` 接口，并使用 `PropertyChanged` 事件对属性值发出信号更改。

## 3.6. 数据绑定和 MVVM

MVVM 模式强制实施三个软件层之间的分离 — XAML 用户界面（称为视图）、基础数据（称为模型）以及视图和模型之间的中介（称为视图模型）。视图和视图模型通常通过 XAML 中定义的数据绑定进行连接。视图的 `BindingContext` 通常是视图模型的实例。

> 重要： .NET MAUI 将绑定更新封送到 UI 线程。使用 MVVM 时，能够从任何线程更新数据绑定视图模型属性，.NET MAUI 的绑定引擎将更新引入 UI 线程。

### 3.6.1. 简单的 MVVM

在 [XAML 标记扩展](https://learn.microsoft.com/en-us/dotnet/maui/xaml/fundamentals/markup-extensions)中，了解了如何定义新的 XML 命名空间声明以允许 XAML 文件引用其他程序集中的类。下面的示例使用 `x:Static` 标记扩展从 `System` 命名空间中的静态 `DateTime.Now` 属性获取当前日期和时间：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=netstandard"
             x:Class="XamlSamples.OneShotDateTimePage"
             Title="One-Shot DateTime Page">

    <VerticalStackLayout BindingContext="{x:Static sys:DateTime.Now}"
                         Spacing="25" Padding="30,0"
                         VerticalOptions="Center" HorizontalOptions="Center">

        <Label Text="{Binding Year, StringFormat='The year is {0}'}" />
        <Label Text="{Binding StringFormat='The month is {0:MMMM}'}" />
        <Label Text="{Binding Day, StringFormat='The day is {0}'}" />
        <Label Text="{Binding StringFormat='The time is {0:T}'}" />

    </VerticalStackLayout>

</ContentPage>
```

在此示例中，检索到的 `DateTime` 值设置为 [StackLayout](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.stacklayout) 上的 `BindingContext`。在元素上设置 `BindingContext` 时，该元素的所有子元素都会继承该元素。这意味着 [StackLayout](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.stacklayout) 的所有子级都具有相同的 `BindingContext`，并且可以包含与该对象的属性的绑定：

![Screenshot of a page displaying the date and time.](https://learn.microsoft.com/en-us/dotnet/maui/xaml/fundamentals/media/mvvm/oneshotdatetime.png)

但是，问题是日期和时间在构造和初始化页面时设置一次，并且永远不会更改。

XAML 页可以显示始终显示当前时间的时钟，但它需要其他代码。当从视觉对象和基础数据之间的属性进行数据绑定时，MVVM 模式是 .NET MAUI 应用的自然选择。当考虑 MVVM 时，模型和视图模型是完全用代码编写的类。视图通常是一个 XAML 文件，该文件通过数据绑定引用视图模型中定义的属性。在 MVVM 中，模型不了解视图模型，视图模型不了解视图。但是，通常将视图模型公开的类型定制为与 UI 关联的类型。

> 注意：在 MVVM 的简单示例中（例如此处所示的示例），通常根本没有模型，并且模式仅涉及与数据绑定链接的视图和视图模型。

下面的示例演示时钟的视图模型，其中包含一个名为 `DateTime` 的单个属性，该属性每秒更新一次：

C#

```c#
using System.ComponentModel;
using System.Runtime.CompilerServices;

namespace XamlSamples;

class ClockViewModel: INotifyPropertyChanged{
    public event PropertyChangedEventHandler PropertyChanged;

    private DateTime _dateTime;
    private Timer _timer;

    public DateTime DateTime{
        get => _dateTime;
        set
        {
            if (_dateTime != value)
            {
                _dateTime = value;
                OnPropertyChanged(); // reports this property
            }
        }
    }

    public ClockViewModel(){
        this.DateTime = DateTime.Now;

        // Update the DateTime property every second.
        _timer = new Timer(new TimerCallback((s) => this.DateTime = DateTime.Now),
                           null, TimeSpan.Zero, TimeSpan.FromSeconds(1));
    }

    ~ClockViewModel() =>
        _timer.Dispose();

    public void OnPropertyChanged([CallerMemberName] string name = "") =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

视图模型通常实现 `INotifyPropertyChanged` 接口，该接口使类能够在其属性之一发生更改时引发 `PropertyChanged` 事件。.NET MAUI 中的数据绑定机制将处理程序附加到此 `PropertyChanged` 事件，以便在属性更改时通知该处理程序，并使用新值更新目标。在前面的代码示例中， `OnPropertyChanged` 处理引发事件，同时自动确定属性源名称： `DateTime` 。

下面的示例演示使用 `ClockViewModel` 的 XAML:

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.ClockPage"
             Title="Clock Page">
    <ContentPage.BindingContext>
        <local:ClockViewModel />
    </ContentPage.BindingContext>

    <Label Text="{Binding DateTime, StringFormat='{0:T}'}"
           FontSize="18"
           HorizontalOptions="Center"
           VerticalOptions="Center" />
</ContentPage>
```

在此示例中，`ClockViewModel` 使用属性元素标记设置为 [ContentPage](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.contentpage) 的 `BindingContext`。或者，代码隐藏文件可以实例化视图模型。

[标签](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label)的 `Text` 属性上的 `Binding `标记扩展设置日期 `DateTime` 属性的格式。以下屏幕截图显示了结果：

![Screenshot of a page displaying the date and time via a viewmodel.](https://learn.microsoft.com/en-us/dotnet/maui/xaml/fundamentals/media/mvvm/clock.png)

此外，还可以通过使用句点分隔属性来访问视图模型的 `DateTime` 属性的各个属性：

```xaml
<Label Text="{Binding DateTime.Second, StringFormat='{0}'}" … >
```

### 3.6.2. 交互式 MVVM

MVVM 通常与基于基础数据模型的交互式视图的双向数据绑定一起使用。

以下示例显示了将 [Color](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.graphics.color) 值转换为色调 Hue 、饱和度 Saturation 和明度 Luminosity 值并再次转换回来的  HslViewModel.xaml 。

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;

namespace XamlSamples;

class HslViewModel: INotifyPropertyChanged{
    public event PropertyChangedEventHandler PropertyChanged;

    private float _hue, _saturation, _luminosity;
    private Color _color;

    public float Hue
    {
        get => _hue;
        set
        {
            if (_hue != value)
                Color = Color.FromHsla(value, _saturation, _luminosity);
        }
    }

    public float Saturation
    {
        get => _saturation;
        set
        {
            if (_saturation != value)
                Color = Color.FromHsla(_hue, value, _luminosity);
        }
    }

    public float Luminosity
    {
        get => _luminosity;
        set
        {
            if (_luminosity != value)
                Color = Color.FromHsla(_hue, _saturation, value);
        }
    }

    public Color Color
    {
        get => _color;
        set
        {
            if (_color != value)
            {
                _color = value;
                _hue = _color.GetHue();
                _saturation = _color.GetSaturation();
                _luminosity = _color.GetLuminosity();

                OnPropertyChanged("Hue");
                OnPropertyChanged("Saturation");
                OnPropertyChanged("Luminosity");
                OnPropertyChanged(); // reports this property
            }
        }
    }

    public void OnPropertyChanged([CallerMemberName] string name = "") =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

在此示例中，对 Hue 、 Saturation 和 Luminosity 属性的更改会导致 Color 属性发生更改，对 Color 属性的更改会导致其他三个属性发生更改。 Color 看起来像是一个无限循环，除了视图模型不会调用 `PropertyChanged` 事件，除非属性已更改。

下面的 XAML 示例包含一个 [BoxView](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.boxview)，其`Color`属性绑定到视图模型的 `Color` 属性，以及绑定到 Hue 、 Saturation 和 Luminosity 属性的三个[滑块](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.slider)视图`Saturation`三个[标签](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label)视图：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.HslColorScrollPage"
             Title="HSL Color Scroll Page">
    <ContentPage.BindingContext>
        <local:HslViewModel Color="Aqua" />
    </ContentPage.BindingContext>

    <VerticalStackLayout Padding="10, 0, 10, 30">
        <BoxView Color="{Binding Color}"
                 HeightRequest="100"
                 WidthRequest="100"
                 HorizontalOptions="Center" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}"
               HorizontalOptions="Center" />
        <Slider Value="{Binding Hue}"
                Margin="20,0,20,0" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}"
               HorizontalOptions="Center" />
        <Slider Value="{Binding Saturation}"
                Margin="20,0,20,0" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}"
               HorizontalOptions="Center" />
        <Slider Value="{Binding Luminosity}"
                Margin="20,0,20,0" />
    </VerticalStackLayout>
</ContentPage>
```

每个 [Label](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.label) 上的绑定是默认的 `OneWay` 。它只需要显示值。但是，每个 [Slider](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.slider) 上的默认绑定是`TwoWay`。这允许从视图模型初始化 [Slider](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.slider) 。实例化视图模型时，其 `Color` 属性设置为 `Aqua` 。 [Slider](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.slider) 中的一个变化设置为视图模型中的属性的一个新值，然后计算新颜色：

![MVVM using two-way data bindings.](https://learn.microsoft.com/en-us/dotnet/maui/xaml/fundamentals/media/mvvm/hslcolorscroll.png)



### 3.6.3. 指挥

有时，应用的需求超出了属性绑定，要求用户启动影响视图模型中某些内容的命令。这些命令通常由按钮单击或手指点击发出信号，传统上，它们在按钮的 `Clicked` [事件或](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button) [TapGestureRecognizer](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.tapgesturerecognizer) 的`Tapped`中的代码隐藏文件中进行处理.

命令接口提供了一种实现命令的替代方法，该方法更适合 MVVM 体系结构。视图模型可以包含命令，这些命令是为响应视图中的特定活动（如[按钮](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button)单击）而执行的方法。数据绑定在这些命令和[按钮](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button)之间定义.

为了允许[按钮](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button)和视图模型之间的数据绑定，[按钮](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button)定义了两个属性：

- [`System.Windows.Input.ICommand`](https://learn.microsoft.com/en-us/dotnet/api/system.windows.input.icommand) 类型的`Command`
- `CommandParameter` `Object`

> 注意：许多其他控件还定义命令 `Command` 和 `CommandParameter` 属性。

[`ICommand`](https://learn.microsoft.com/en-us/dotnet/api/system.windows.input.icommand) 接口在 [System.Windows.Input](https://learn.microsoft.com/en-us/dotnet/api/system.windows.input) 命名空间中定义，由两个方法和一个事件组成：

- `void Execute(object arg)`
- `bool CanExecute(object arg)`
- `event EventHandler CanExecuteChanged`

视图模型可以定义 `ICommand` 类型的属性。然后，可以将这些属性绑定到每个 [Button](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button) 或其他元素的 `Command` 属性，或者绑定到实现此接口的自定义视图。可以选择设置 `CommandParameter` 属性来标识绑定到此视图模型属性的单个 [Button](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button) 对象（或其他元素）。在内部，每当用户点击按钮时，[按钮](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button)都会调用 Execute 方法，将其`CommandParameter`传递给 `Execute` 方法。

`CanExecute` 方法和 `CanExecuteChanged` 事件用于点击当前 [Button](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button) 可能无效的情况，在这种情况下，[按钮](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button)应自行禁用。[Button](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button) 在首次设置 `Command` 属性时以及引发 CanExecuteChanged 事件时调用`CanExecute` ，如果 `CanExecute` 返回 `false`，则 [Button](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button) 将禁用自身，并且不会生成 `Execute` 调用。

可以使用 .NET MAUI 中包含的 Command 或 `Command<T>` 类来实现 `ICommand` 接口。`Command`这两个类定义了几个构造函数和一个 `ChangeCanExecute` 方法，视图模型可以调用该方法来强制 `Command` 对象引发 `CanExecuteChanged` 事件。

以下示例显示了用于输入电话号码的简单键盘的视图模型：

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;
using System.Windows.Input;

namespace XamlSamples;

class KeypadViewModel: INotifyPropertyChanged{
    public event PropertyChangedEventHandler PropertyChanged;

    private string _inputString = "";
    private string _displayText = "";
    private char[] _specialChars = { '*', '#' };

    public ICommand AddCharCommand { get; private set; }
    public ICommand DeleteCharCommand { get; private set; }

    public string InputString
    {
        get => _inputString;
        private set
        {
            if (_inputString != value)
            {
                _inputString = value;
                OnPropertyChanged();
                DisplayText = FormatText(_inputString);

                // Perhaps the delete button must be enabled/disabled.
                ((Command)DeleteCharCommand).ChangeCanExecute();
            }
        }
    }

    public string DisplayText
    {
        get => _displayText;
        private set
        {
            if (_displayText != value)
            {
                _displayText = value;
                OnPropertyChanged();
            }
        }
    }

    public KeypadViewModel(){
        // Command to add the key to the input string
        AddCharCommand = new Command<string>((key) => InputString += key);

        // Command to delete a character from the input string when allowed
        DeleteCharCommand =
            new Command(
                // Command will strip a character from the input string
                () => InputString = InputString.Substring(0, InputString.Length - 1),

                // CanExecute is processed here to return true when there's something to delete
                () => InputString.Length > 0
            );
    }

    string FormatText(string str){
        bool hasNonNumbers = str.IndexOfAny(_specialChars) != -1;
        string formatted = str;

        // Format the string based on the type of data and the length
        if (hasNonNumbers || str.Length < 4 || str.Length > 10){
            // Special characters exist, or the string is too small or large for special formatting
            // Do nothing
        }

        else if (str.Length < 8)
            formatted = string.Format("{0}-{1}", str.Substring(0, 3), str.Substring(3));

        else
            formatted = string.Format("({0}) {1}-{2}", str.Substring(0, 3), str.Substring(3, 3), str.Substring(6));

        return formatted;
    }


    public void OnPropertyChanged([CallerMemberName] string name = "") =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(name));
}
```

在此示例中，命令的 `Execute` 和 `CanExecute` 方法在构造函数中定义为 lambda 函数。视图模型假定 `AddCharCommand` 属性绑定到多个按钮（或具有命令接口的任何其他控件）的 Command 属性，每个按钮都由  `CommandParameter` 标识。这些按钮将字符添加到 `InputString` 属性，然后将该属性格式化为 `DisplayText` 属性的电话号码。还有第二个类型`ICommand` 的属性，名为 `DeleteCharCommand`。这绑定到后间距按钮，但没有要删除的字符，则应禁用该按钮。

KeypadViewModel.xaml 如下：

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.KeypadPage"
             Title="Keypad Page">
    <ContentPage.BindingContext>
        <local:KeypadViewModel />
    </ContentPage.BindingContext>

    <Grid HorizontalOptions="Center" VerticalOptions="Center">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
            <ColumnDefinition Width="80" />
        </Grid.ColumnDefinitions>

        <Label Text="{Binding DisplayText}"
               Margin="0,0,10,0" FontSize="20" LineBreakMode="HeadTruncation"
               VerticalTextAlignment="Center" HorizontalTextAlignment="End"
               Grid.ColumnSpan="2" />

        <Button Text="&#x21E6;" Command="{Binding DeleteCharCommand}" Grid.Column="2"/>

        <Button Text="1" Command="{Binding AddCharCommand}" CommandParameter="1" Grid.Row="1" />
        <Button Text="2" Command="{Binding AddCharCommand}" CommandParameter="2" Grid.Row="1" Grid.Column="1" />
        <Button Text="3" Command="{Binding AddCharCommand}" CommandParameter="3" Grid.Row="1" Grid.Column="2" />

        <Button Text="4" Command="{Binding AddCharCommand}" CommandParameter="4" Grid.Row="2" />
        <Button Text="5" Command="{Binding AddCharCommand}" CommandParameter="5" Grid.Row="2" Grid.Column="1" />
        <Button Text="6" Command="{Binding AddCharCommand}" CommandParameter="6" Grid.Row="2" Grid.Column="2" />

        <Button Text="7" Command="{Binding AddCharCommand}" CommandParameter="7" Grid.Row="3" />
        <Button Text="8" Command="{Binding AddCharCommand}" CommandParameter="8" Grid.Row="3" Grid.Column="1" />
        <Button Text="9" Command="{Binding AddCharCommand}" CommandParameter="9" Grid.Row="3" Grid.Column="2" />

        <Button Text="*" Command="{Binding AddCharCommand}" CommandParameter="*" Grid.Row="4" />
        <Button Text="0" Command="{Binding AddCharCommand}" CommandParameter="0" Grid.Row="4" Grid.Column="1" />
        <Button Text="#" Command="{Binding AddCharCommand}" CommandParameter="#" Grid.Row="4" Grid.Column="2" />
    </Grid>
</ContentPage>
```

在此示例中，第一个[按钮](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button)的 `Command` 属性绑定到 `DeleteCharCommand` 。其他按钮通过显示在 [Button](https://learn.microsoft.com/en-us/dotnet/api/microsoft.maui.controls.button) 上的相同字符的 `CommandParameter` 绑定到 `AddCharCommand` 。

![Screenshot of a calculator using MVVM and commands.](https://learn.microsoft.com/en-us/dotnet/maui/xaml/fundamentals/media/mvvm/keypad.png)