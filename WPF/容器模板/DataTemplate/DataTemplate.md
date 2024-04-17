
# 資料範本化概觀

- 發行項
- 2023/10/18
- 2 位參與者

意見反應

## 本文內容

1. [必要條件](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#prerequisites)
2. [資料範本化基本概念](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#data-templating-basics)
3. [加入更多內容至 DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#adding-more-to-the-datatemplate)
4. [根據資料物件的屬性選擇 DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#choosing-a-datatemplate-based-on-properties-of-the-data-object)

顯示其他 3 個

WPF 資料範本化模型對於資料呈現方式的定義，具有相當大的彈性。 WPF 控制項的內建功能支援自訂資料呈現方式。 本主題先示範如何定義 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) ，然後介紹其他資料範本化功能，例如根據自訂邏輯選取範本，以及顯示階層式資料的支援。

[](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#prerequisites)

## 必要條件

本主題著重資料範本化功能，而不是資料繫結概念簡介。 如需基本資料繫結概念的資訊，請參閱[資料繫結概觀](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-binding-overview?view=netframeworkdesktop-4.8)。

[DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 是關於資料的呈現方式，而且是 WPF 樣式和範本化模型所提供的許多功能之一。 如需 WPF 樣式和範本化模型的簡介，例如如何使用 [Style](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.style) 在控制項上設定屬性，請參閱 [樣式和範本化](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/controls/styles-templates-overview?view=netframeworkdesktop-4.8) 主題。

此外，請務必瞭解 `Resources` ，這基本上是讓 和 等 [Style](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.style)[DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 物件可重複使用的物件。 如需詳細資訊，請參閱 [XAML 資源](https://learn.microsoft.com/zh-tw/dotnet/desktop-wpf/fundamentals/xaml-resources-define)。

[](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#data-templating-basics)

## 資料範本化基本概念

為了示範為什麼 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 很重要，讓我們逐步解說資料系結範例。 在此範例中，我們有 [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) 系結至 物件清單的 `Task` 。 每個 `Task` 物件各有一個 `TaskName` (字串)、`Description` (字串)、`Priority` (整數)，和一個 `TaskType`型別的屬性，該型別是含有值 `Home` 和 `Work` 的 `Enum`。

XAML複製

```
<Window x:Class="SDKSample.Window1"
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
  xmlns:local="clr-namespace:SDKSample"
  Title="Introduction to Data Templating Sample">
  <Window.Resources>
    <local:Tasks x:Key="myTodoList"/>
```

XAML複製

```

</Window.Resources>
  <StackPanel>
    <TextBlock Name="blah" FontSize="20" Text="My Task List:"/>
    <ListBox Width="400" Margin="10"
             ItemsSource="{Binding Source={StaticResource myTodoList}}"/>
```

XAML複製

```
  </StackPanel>
</Window>
```

[](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#without-a-datatemplate)

### 沒有 DataTemplate

[DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate)如果沒有 ，我們 [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) 目前看起來會像這樣：

![Screenshot of the Introduction to Data Templating Sample window showing the My Task List ListBox displaying the string representation SDKSample.Task for each source object.](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/media/datatemplatingintro-fig1.png?view=netframeworkdesktop-4.8 "DataTemplatingIntro_fig1")

發生的情況是，如果沒有任何特定指示， [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) 預設會在嘗試在集合中顯示物件時呼叫 `ToString` 。 因此，如果 `Task` 物件覆寫 `ToString` 方法，則 會顯示 [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) 基礎集合中每個來源物件的字串表示。

例如，如果 `Task` 類別以此方式覆寫 `ToString` 方法，而其中 `name` 是 `TaskName`屬性的欄位︰

C#複製

```
public override string ToString()
{
    return name.ToString();
}
```

[ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox)然後看起來如下：

![Screenshot of the Introduction to Data Templating Sample window showing the My Task List ListBox displaying a list of tasks.](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/media/datatemplatingintro-fig2.png?view=netframeworkdesktop-4.8 "DataTemplatingIntro_fig2")

不過，這是有限制的，而且缺乏彈性。 此外，如果您要系結至 XML 資料，您將無法覆寫 `ToString` 。

[](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#defining-a-simple-datatemplate)

### 定義簡單的 DataTemplate

解決方案是定義 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 。 其中一種方法是將 的 [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) 屬性設定 [ItemTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol.itemtemplate) 為 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 。 您在 中指定的 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 內容會成為資料物件的視覺結構。 以下是 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 相當簡單的。 我們會提供指示，指出每個專案在 內顯示為三 [TextBlock](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.textblock) 個 [StackPanel](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.stackpanel) 元素。 每個 [TextBlock](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.textblock) 元素都會系結至 類別的 `Task` 屬性。

XAML複製

```
<ListBox Width="400" Margin="10"
         ItemsSource="{Binding Source={StaticResource myTodoList}}">
   <ListBox.ItemTemplate>
     <DataTemplate>
       <StackPanel>
         <TextBlock Text="{Binding Path=TaskName}" />
         <TextBlock Text="{Binding Path=Description}"/>
         <TextBlock Text="{Binding Path=Priority}"/>
       </StackPanel>
     </DataTemplate>
   </ListBox.ItemTemplate>
 </ListBox>
```

本主題中範例的基礎資料是 CLR 物件的集合。 如果您要系結至 XML 資料，則基本概念相同，但語法稍有差異。 例如，您不會設定 `Path=TaskName` 為 ，而是將 設定 [XPath](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.data.binding.xpath) 為 `@TaskName` （如果 `TaskName` 是 XML 節點的屬性）。

現在，我們的 [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) 外觀如下：

![Screenshot of the Introduction to Data Templating Sample window showing the My Task List ListBox displaying the tasks as TextBlock elements.](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/media/datatemplatingintro-fig3.png?view=netframeworkdesktop-4.8 "DataTemplatingIntro_fig3")

[](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#creating-the-datatemplate-as-a-resource)

### 將 DataTemplate 建立為資源

在上述範例中，我們定義了 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 內嵌。 更常見的是將它定義於資源區段中以成為可重複使用的物件，如下列範例所述：

XAML複製

```
<Window.Resources>
```

XAML複製

```
<DataTemplate x:Key="myTaskTemplate">
  <StackPanel>
    <TextBlock Text="{Binding Path=TaskName}" />
    <TextBlock Text="{Binding Path=Description}"/>
    <TextBlock Text="{Binding Path=Priority}"/>
  </StackPanel>
</DataTemplate>
```

XAML複製

```
</Window.Resources>
```

現在您可以使用 `myTaskTemplate` 做為資源，如下列範例所示︰

XAML複製

```
<ListBox Width="400" Margin="10"
         ItemsSource="{Binding Source={StaticResource myTodoList}}"
         ItemTemplate="{StaticResource myTaskTemplate}"/>
```

因為 `myTaskTemplate` 是資源，所以您現在可以將它用於具有採用 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 類型之屬性的其他控制項上。 如上所示，針對 [ItemsControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol) 物件，例如 [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) ，它是 [ItemTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol.itemtemplate) 屬性。 如果是 [ContentControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.contentcontrol) 物件，它是 [ContentTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.contentcontrol.contenttemplate) 屬性。

[](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#the-datatype-property)

### DataType 屬性

類別 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 的屬性 [DataType](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate.datatype) 與 類別的 [Style](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.style) 屬性非常類似 [TargetType](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.style.targettype) 。 因此，您可以執行下列動作，而不是在上述範例中指定 `x:Key`[DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 的 ：

XAML複製

```
<DataTemplate DataType="{x:Type local:Task}">
  <StackPanel>
    <TextBlock Text="{Binding Path=TaskName}" />
    <TextBlock Text="{Binding Path=Description}"/>
    <TextBlock Text="{Binding Path=Priority}"/>
  </StackPanel>
</DataTemplate>
```

這 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 會自動套用至所有 `Task` 物件。 請注意，在此情況下，`x:Key` 是隱含設定的。 因此，如果您指派這個 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate)`x:Key` 值，則會覆寫隱含 `x:Key` 的 ， [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 而且不會自動套用 。

如果您要將 系結 [ContentControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.contentcontrol) 至 物件的集合 `Task` ， [ContentControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.contentcontrol) 則 不會自動使用上述 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 。 這是因為 上的 [ContentControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.contentcontrol) 系結需要更多資訊來區別您要系結至整個集合或個別物件。 [ContentControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.contentcontrol)如果您要追蹤類型的選取 [ItemsControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol) 範圍，您可以將系結的 [ContentControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.contentcontrol) 屬性設定 [Path](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.data.binding.path) 為 「 `/` 」，以指出您對目前專案感興趣。 如需範例，請參閱[繫結至集合並根據選取項目顯示資訊](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/how-to-bind-to-a-collection-and-display-information-based-on-selection?view=netframeworkdesktop-4.8)。 否則，您必須藉由設定 [ContentTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.contentcontrol.contenttemplate) 屬性來明確指定 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 。

當您有 [CompositeCollection](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.data.compositecollection) 不同類型的資料物件時，屬性 [DataType](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate.datatype) 特別有用。 如需範例，請參閱[實作 CompositeCollection](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/how-to-implement-a-compositecollection?view=netframeworkdesktop-4.8)。

[](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#adding-more-to-the-datatemplate)

## 加入更多內容至 DataTemplate

目前資料是以所需的資訊出現，不過還有很大的改進空間。 讓我們藉由新增 、 [Border](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.border) 、 [Grid](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.grid) 和 一些 [TextBlock](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.textblock) 描述所顯示資料的元素，來改善簡報。

XAML複製

```

<DataTemplate x:Key="myTaskTemplate">
  <Border Name="border" BorderBrush="Aqua" BorderThickness="1"
          Padding="5" Margin="5">
    <Grid>
      <Grid.RowDefinitions>
        <RowDefinition/>
        <RowDefinition/>
        <RowDefinition/>
      </Grid.RowDefinitions>
      <Grid.ColumnDefinitions>
        <ColumnDefinition />
        <ColumnDefinition />
      </Grid.ColumnDefinitions>
      <TextBlock Grid.Row="0" Grid.Column="0" Text="Task Name:"/>
      <TextBlock Grid.Row="0" Grid.Column="1" Text="{Binding Path=TaskName}" />
      <TextBlock Grid.Row="1" Grid.Column="0" Text="Description:"/>
      <TextBlock Grid.Row="1" Grid.Column="1" Text="{Binding Path=Description}"/>
      <TextBlock Grid.Row="2" Grid.Column="0" Text="Priority:"/>
      <TextBlock Grid.Row="2" Grid.Column="1" Text="{Binding Path=Priority}"/>
    </Grid>
  </Border>
```

XAML複製

```
</DataTemplate>
```

下列螢幕擷取畫面顯示 [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) 已修改 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 的 ：

![Screenshot of the Introduction to Data Templating Sample window showing the My Task List ListBox with the modified DataTemplate.](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/media/datatemplatingintro-fig4.png?view=netframeworkdesktop-4.8 "DataTemplatingIntro_fig4")

我們可以將 設定 [HorizontalContentAlignment](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.control.horizontalcontentalignment) 為 [Stretch](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.horizontalalignment#system-windows-horizontalalignment-stretch) ， [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) 以確保專案的寬度佔用整個空間：

XAML複製

```
<ListBox Width="400" Margin="10"
     ItemsSource="{Binding Source={StaticResource myTodoList}}"
     ItemTemplate="{StaticResource myTaskTemplate}" 
     HorizontalContentAlignment="Stretch"/>
```

將 [HorizontalContentAlignment](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.control.horizontalcontentalignment) 屬性設定為 [Stretch](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.horizontalalignment#system-windows-horizontalalignment-stretch) ， [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) 現在看起來會像這樣：

![Screenshot of the Introduction to Data Templating Sample window showing the My Task List ListBox stretched to fit the screen horizontally.](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/media/datatemplatingintro-fig5.png?view=netframeworkdesktop-4.8 "DataTemplatingIntro_fig5")

[](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#use-datatriggers-to-apply-property-values)

### 使用 DataTriggers 套用屬性值

目前的展示方式並無法分辨出 `Task` 是家務還是公司的工作。 前面提過，`Task` 物件有一個型別為 `TaskType` 的 `TaskType` 屬性，這是具有 `Home` 和 `Work` 這兩個值的列舉。

在下列範例中，如果 屬性是 ，則會 [DataTrigger](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatrigger) 將名為 `border``Yellow` 的專案設定 [BorderBrush](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.border.borderbrush) 為 `TaskType.Home` 。 `TaskType`

XAML複製

```
<DataTemplate x:Key="myTaskTemplate">
```

XAML複製

```
<DataTemplate.Triggers>
  <DataTrigger Binding="{Binding Path=TaskType}">
    <DataTrigger.Value>
      <local:TaskType>Home</local:TaskType>
    </DataTrigger.Value>
    <Setter TargetName="border" Property="BorderBrush" Value="Yellow"/>
  </DataTrigger>
</DataTemplate.Triggers>
```

XAML複製

```
</DataTemplate>
```

現在應用程式看起來就像下面這樣。 家務會用黃色框線框住，而公司工作則有水藍色框線框住：

![Screenshot of the Introduction to Data Templating Sample window showing the My Task List ListBox with the home and office task borders highlighted in color.](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/media/datatemplatingintro-fig6.png?view=netframeworkdesktop-4.8 "DataTemplatingIntro_fig6")

在此範例中，會 [DataTrigger](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatrigger) 使用 [Setter](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.setter) 來設定 屬性值。 觸發程式類別也有 [EnterActions](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.triggerbase.enteractions) 和 [ExitActions](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.triggerbase.exitactions) 屬性，可讓您啟動一組動作，例如動畫。 此外，還有一個 [MultiDataTrigger](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.multidatatrigger) 類別可讓您根據多個資料系結屬性值來套用變更。

達成相同效果的替代方式是將 屬性系結 [BorderBrush](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.border.borderbrush) 至 `TaskType` 屬性，並使用值轉換器根據值傳回色彩 `TaskType` 。 使用轉換器建立上述效果，就效能上來說會快些。 此外，建立自己的轉換器能提供更多的彈性，因為您可以提供自己的邏輯。 最後，要選擇使用哪種方式，取決於個別情況和您的偏好而定。 如需如何撰寫轉換器的資訊，請參閱 [IValueConverter](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.data.ivalueconverter) 。

[](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#what-belongs-in-a-datatemplate)

### 哪些內容屬於 DataTemplate 的範圍

在上一個範例中，我們使用 屬性將觸發程式放在 內 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate)[DataTemplate.Triggers](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate.triggers) 。 [Setter](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.setter)觸發程式的 會設定 中專案 （專案 [Border](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.border) ） [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 的 屬性值。 不過，如果您關心的屬性 `Setters` 不是目前 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 內之元素的屬性，則使用 類別的屬性可能更適合設定 [ListBoxItem](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listboxitem)[Style](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.style) 屬性（如果您要系結的控制項是 。 [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) 例如，如果您想要 [Trigger](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.trigger) 在滑鼠指向專案時以動畫顯示 [Opacity](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.uielement.opacity) 專案的值，您可以在樣式中 [ListBoxItem](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listboxitem) 定義觸發程式。 如需範例，請參閱[樣式設定和範本化範例的簡介](https://github.com/Microsoft/WPF-Samples/tree/master/Styles%20&%20Templates/IntroToStylingAndTemplating)。

一般而言，請記住 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) ，正在套用至每個產生的 [ListBoxItem](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listboxitem) 專案（如需實際套用方式和位置的詳細資訊，請參閱 [ItemTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol.itemtemplate) 頁面。 您 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 只關心資料物件的呈現和外觀。 在大部分情況下，呈現的所有其他層面，例如選取專案時的外觀，或設定項目的方式 [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) ，不屬於 的定義 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 。 如需範例，請參閱 [ItemsControl 的樣式設定和範本化](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#DataTemplating_ItemsControl)一節。

[](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#choosing-a-datatemplate-based-on-properties-of-the-data-object)

## 根據資料物件的屬性選擇 DataTemplate

在 [DataType 屬性](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#Styling_DataType)一節中提到過，您可以為不同的資料物件定義不同的資料範本。 當您有 [CompositeCollection](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.data.compositecollection) 不同類型或具有不同類型專案的集合時，這特別有用。 在 [ [使用 DataTriggers 套用屬性值](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#DataTrigger_to_Apply_Property_Values) ] 區段中，我們已經顯示，如果您有相同類型的資料物件集合，您可以建立 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) ，然後使用觸發程式根據每個資料物件的屬性值來套用變更。 不過，觸發程序雖然可以讓您套用屬性值或啟動動畫，但無法提供重新建構資料物件結構的彈性。 有些案例可能會要求您為相同類型但具有不同屬性的資料物件建立不同的 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 。

例如，當 `Task` 物件有一個 `Priority` 值為 `1` 時，您可能會想讓它看起來完全不同，以便提醒自己。 在此情況下，您會為高優先順序 `Task` 物件的顯示建立 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 。 讓我們將下列內容 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 新增至 resources 區段：

XAML複製

```
<DataTemplate x:Key="importantTaskTemplate">
  <DataTemplate.Resources>
    <Style TargetType="TextBlock">
      <Setter Property="FontSize" Value="20"/>
    </Style>
  </DataTemplate.Resources>
  <Border Name="border" BorderBrush="Red" BorderThickness="1"
          Padding="5" Margin="5">
    <DockPanel HorizontalAlignment="Center">
      <TextBlock Text="{Binding Path=Description}" />
      <TextBlock>!</TextBlock>
    </DockPanel>
  </Border>
</DataTemplate>
```

此範例使用 [DataTemplate.Resources](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.frameworktemplate.resources) 屬性。 在該區段中定義的資源會由 內的 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 專案共用。

若要提供邏輯，以根據 `Priority` 資料物件的值選擇 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 要使用的邏輯，請建立 的 [DataTemplateSelector](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.datatemplateselector) 子類別並覆寫 [SelectTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.datatemplateselector.selecttemplate) 方法。 在下列範例中 [SelectTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.datatemplateselector.selecttemplate) ，方法會提供邏輯，根據 屬性的值 `Priority` 傳回適當的範本。 要傳回的範本位於 enveloping [Window](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.window) 元素的資源中。

C#複製

```
using System.Windows;
using System.Windows.Controls;

namespace SDKSample
{
    public class TaskListDataTemplateSelector : DataTemplateSelector
    {
        public override DataTemplate
            SelectTemplate(object item, DependencyObject container)
        {
            FrameworkElement element = container as FrameworkElement;

            if (element != null && item != null && item is Task)
            {
                Task taskitem = item as Task;

                if (taskitem.Priority == 1)
                    return
                        element.FindResource("importantTaskTemplate") as DataTemplate;
                else
                    return
                        element.FindResource("myTaskTemplate") as DataTemplate;
            }

            return null;
        }
    }
}
```

然後，就可以宣告`TaskListDataTemplateSelector` 為資源：

XAML複製

```
<Window.Resources>
```

XAML複製

```
<local:TaskListDataTemplateSelector x:Key="myDataTemplateSelector"/>
```

XAML複製

```
</Window.Resources>
```

若要使用範本選取器資源，請將它指派給 [ItemTemplateSelector](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol.itemtemplateselector) 的 [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) 屬性。 會 [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) 針對基礎集合中的每個專案呼叫 [SelectTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.datatemplateselector.selecttemplate) 的 方法 `TaskListDataTemplateSelector` 。 該呼叫會將資料物件當做項目參數傳遞。 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate)方法所傳回的 ，接著會套用至該資料物件。

XAML複製

```
<ListBox Width="400" Margin="10"
         ItemsSource="{Binding Source={StaticResource myTodoList}}"
         ItemTemplateSelector="{StaticResource myDataTemplateSelector}"
         HorizontalContentAlignment="Stretch"/>
```

使用範本選取器就地， [ListBox](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.listbox) 現在會顯示如下：

![Screenshot of Introduction to Data Templating Sample window showing the My Task List ListBox with the Priority 1 tasks prominently displayed with a red border.](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/media/datatemplatingintro-fig7.png?view=netframeworkdesktop-4.8 "DataTemplatingIntro_fig7")

這個範例的討論到此結束。 如需完整範例，請參閱[資料範本化範例簡介](https://github.com/Microsoft/WPF-Samples/tree/master/Data%20Binding/DataTemplatingIntro)。

[](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#styling-and-templating-an-itemscontrol)

## ItemsControl 的樣式設定和範本化

雖然 [ItemsControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol) 不是唯一可以使用 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 的控制項類型，但系結 [ItemsControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol) 至集合是很常見的案例。 在 DataTemplate [](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#what_belongs_in_datatemplate)的 [屬於什麼] 區段中，我們已討論您的 [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 定義應該只關注資料的呈現。 若要知道何時不適合使用 ， [DataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.datatemplate) 請務必瞭解 所提供的 [ItemsControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol) 不同樣式和範本屬性。 下列範例是設計來說明每個屬性的功能。 此範例中的 系 [ItemsControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol) 結至與 `Tasks` 上一個範例相同的集合。 為方便示範，這個範例中的樣式和範本都是內嵌宣告的。

XAML複製

```
<ItemsControl Margin="10"
              ItemsSource="{Binding Source={StaticResource myTodoList}}">
  <!--The ItemsControl has no default visual appearance.
      Use the Template property to specify a ControlTemplate to define
      the appearance of an ItemsControl. The ItemsPresenter uses the specified
      ItemsPanelTemplate (see below) to layout the items. If an
      ItemsPanelTemplate is not specified, the default is used. (For ItemsControl,
      the default is an ItemsPanelTemplate that specifies a StackPanel.-->
  <ItemsControl.Template>
    <ControlTemplate TargetType="ItemsControl">
      <Border BorderBrush="Aqua" BorderThickness="1" CornerRadius="15">
        <ItemsPresenter/>
      </Border>
    </ControlTemplate>
  </ItemsControl.Template>
  <!--Use the ItemsPanel property to specify an ItemsPanelTemplate
      that defines the panel that is used to hold the generated items.
      In other words, use this property if you want to affect
      how the items are laid out.-->
  <ItemsControl.ItemsPanel>
    <ItemsPanelTemplate>
      <WrapPanel />
    </ItemsPanelTemplate>
  </ItemsControl.ItemsPanel>
  <!--Use the ItemTemplate to set a DataTemplate to define
      the visualization of the data objects. This DataTemplate
      specifies that each data object appears with the Proriity
      and TaskName on top of a silver ellipse.-->
  <ItemsControl.ItemTemplate>
    <DataTemplate>
      <DataTemplate.Resources>
        <Style TargetType="TextBlock">
          <Setter Property="FontSize" Value="18"/>
          <Setter Property="HorizontalAlignment" Value="Center"/>
        </Style>
      </DataTemplate.Resources>
      <Grid>
        <Ellipse Fill="Silver"/>
        <StackPanel>
          <TextBlock Margin="3,3,3,0"
                     Text="{Binding Path=Priority}"/>
          <TextBlock Margin="3,0,3,7"
                     Text="{Binding Path=TaskName}"/>
        </StackPanel>
      </Grid>
    </DataTemplate>
  </ItemsControl.ItemTemplate>
  <!--Use the ItemContainerStyle property to specify the appearance
      of the element that contains the data. This ItemContainerStyle
      gives each item container a margin and a width. There is also
      a trigger that sets a tooltip that shows the description of
      the data object when the mouse hovers over the item container.-->
  <ItemsControl.ItemContainerStyle>
    <Style>
      <Setter Property="Control.Width" Value="100"/>
      <Setter Property="Control.Margin" Value="5"/>
      <Style.Triggers>
        <Trigger Property="Control.IsMouseOver" Value="True">
          <Setter Property="Control.ToolTip"
                  Value="{Binding RelativeSource={x:Static RelativeSource.Self},
                          Path=Content.Description}"/>
        </Trigger>
      </Style.Triggers>
    </Style>
  </ItemsControl.ItemContainerStyle>
</ItemsControl>
```

下圖是範例呈現的畫面：

![ItemsControl example screenshot](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/media/databinding-itemscontrolproperties.png?view=netframeworkdesktop-4.8 "DataBinding_ItemsControlProperties")

請注意，您可以使用 ，而不是使用 [ItemTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol.itemtemplate)[ItemTemplateSelector](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol.itemtemplateselector) 。 請參閱上一節中的範例。 同樣地，您可以選擇使用 ，而不是使用 [ItemContainerStyle](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol.itemcontainerstyle)[ItemContainerStyleSelector](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol.itemcontainerstyleselector) 。

此處未顯示 之 的兩個其他樣式相關屬性 [ItemsControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol) 為 [GroupStyle](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol.groupstyle) 和 [GroupStyleSelector](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.itemscontrol.groupstyleselector) 。

[](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#support-for-hierarchical-data)

## 支援階層式資料

到目前為止，我們只討論了如何繫結和顯示單一集合。 有時候集合之中可能還有其他集合。 類別 [HierarchicalDataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.hierarchicaldatatemplate) 的設計目的是要與類型搭配 [HeaderedItemsControl](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.controls.headereditemscontrol) 使用，以顯示這類資料。 在下列範例中，`ListLeagueList` 是 `League` 物件的清單。 每個 `League` 物件都有一個 `Name` 和一組 `Division` 物件集合。 每一個 `Division` 都有一個 `Name` 和 `Team` 物件的集合，並且每一個 `Team` 物件都有一個 `Name`。

XAML複製

```
<Window x:Class="SDKSample.Window1"
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
  Title="HierarchicalDataTemplate Sample"
  xmlns:src="clr-namespace:SDKSample">
  <DockPanel>
    <DockPanel.Resources>
      <src:ListLeagueList x:Key="MyList"/>

      <HierarchicalDataTemplate DataType    = "{x:Type src:League}"
                                ItemsSource = "{Binding Path=Divisions}">
        <TextBlock Text="{Binding Path=Name}"/>
      </HierarchicalDataTemplate>

      <HierarchicalDataTemplate DataType    = "{x:Type src:Division}"
                                ItemsSource = "{Binding Path=Teams}">
        <TextBlock Text="{Binding Path=Name}"/>
      </HierarchicalDataTemplate>

      <DataTemplate DataType="{x:Type src:Team}">
        <TextBlock Text="{Binding Path=Name}"/>
      </DataTemplate>
    </DockPanel.Resources>

    <Menu Name="menu1" DockPanel.Dock="Top" Margin="10,10,10,10">
        <MenuItem Header="My Soccer Leagues"
                  ItemsSource="{Binding Source={StaticResource MyList}}" />
    </Menu>

    <TreeView>
      <TreeViewItem ItemsSource="{Binding Source={StaticResource MyList}}" Header="My Soccer Leagues" />
    </TreeView>

  </DockPanel>
</Window>
```

此範例示範如何使用 [HierarchicalDataTemplate](https://learn.microsoft.com/zh-tw/dotnet/api/system.windows.hierarchicaldatatemplate) ，您可以輕鬆地顯示包含其他清單的清單資料。 以下是範例的螢幕擷取畫面。

![HierarchicalDataTemplate sample screenshot](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/media/databinding-hierarchicaldatatemplate.png?view=netframeworkdesktop-4.8 "DataBinding_HierarchicalDataTemplate")

[](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-templating-overview?view=netframeworkdesktop-4.8#see-also)

## 另請參閱

- [資料繫結](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/advanced/optimizing-performance-data-binding?view=netframeworkdesktop-4.8)
- [尋找 DataTemplate 產生的元素](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/how-to-find-datatemplate-generated-elements?view=netframeworkdesktop-4.8)
- [設定樣式和範本](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/controls/styles-templates-overview?view=netframeworkdesktop-4.8)
- [資料繫結概觀](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/data/data-binding-overview?view=netframeworkdesktop-4.8)
- [GridView 資料行標頭樣式和範本概觀](https://learn.microsoft.com/zh-tw/dotnet/desktop/wpf/controls/gridview-column-header-styles-and-templates-overview?view=netframeworkdesktop-4.8)