
從提供的原始碼中，我們可以看到 `DrawerHost` 類別擴展了 `ContentControl` 並使用了多個 `TemplateVisualState` 和 `TemplatePart` 屬性來定義不同的視覺狀態和模板部分。此類別主要用於實現一個具有多個抽屜（Drawer）的容器控件，每個抽屜可以獨立開啟和關閉，支持上、下、左、右四個方向。下面是 `DrawerHost` 類別中公開的屬性和方法的分析及其使用範例。

### 公開屬性及方法

1. **公開屬性：**
   - `OverlayBackground`: 控制抽屜開啟時遮罩的背景。
   - `OpenMode`: 控制抽屜的開啟模式。
   - 對於每個方向（上、下、左、右）的抽屜，有以下屬性：
     - `*DrawerContent`: 抽屜內容。
     - `*DrawerContentTemplate`: 抽屜內容的模板。
     - `*DrawerContentTemplateSelector`: 決定使用哪個模板渲染內容。
     - `*DrawerContentStringFormat`: 格式化內容的字符串。
     - `*DrawerBackground`: 抽屜背景。
     - `Is*DrawerOpen`: 控制抽屜是否開啟。
     - `*DrawerCloseOnClickAway`: 控制點擊抽屜外部是否關閉抽屜。
     - `*DrawerCornerRadius`: 抽屜的圓角。

2. **公開方法：**
   - `OnApplyTemplate()`: 應用模板時的處理。
   - `OnDrawerOpened(DrawerOpenedEventArgs eventArgs)`: 抽屜開啟時的回調。
   - `OnDrawerClosing(DrawerClosingEventArgs eventArgs)`: 抽屜關閉時的回調。

### 使用範例

#### XAML:

```xaml
<materialDesign:DrawerHost x:Name="MyDrawerHost" OverlayBackground="LightGray">
    <materialDesign:DrawerHost.LeftDrawerContent>
        <TextBlock Text="左側抽屜內容"/>
    </materialDesign:DrawerHost.LeftDrawerContent>
    <materialDesign:DrawerHost.RightDrawerContent>
        <TextBlock Text="右側抽屜內容"/>
    </materialDesign:DrawerHost.RightDrawerContent>
    <Button Content="開啟左側抽屜" Command="{x:Static materialDesign:DrawerHost.OpenDrawerCommand}" CommandParameter="Left"/>
    <Button Content="開啟右側抽屜" Command="{x:Static materialDesign:DrawerHost.OpenDrawerCommand}" CommandParameter="Right"/>
</materialDesign:DrawerHost>
```

#### XAML.CS:

```csharp
public MainWindow()
{
    InitializeComponent();
    // 訂閱抽屜開啟和關閉事件
    MyDrawerHost.DrawerOpened += MyDrawerHost_DrawerOpened;
    MyDrawerHost.DrawerClosing += MyDrawerHost_DrawerClosing;
}

private void MyDrawerHost_DrawerOpened(object sender, DrawerOpenedEventArgs e)
{
    MessageBox.Show($"抽屜 {e.Dock} 已開啟");
}

private void MyDrawerHost_DrawerClosing(object sender, DrawerClosingEventArgs e)
{
    MessageBox.Show($"抽屜 {e.Dock} 正在關閉");
}
```

這個範例展示了如何在 XAML 中定義一個 `DrawerHost` 並為其左側和右側設置內容。同時，在代碼後面訂閱了抽屜開啟和關閉的事件，並通過彈窗顯示相應信息。