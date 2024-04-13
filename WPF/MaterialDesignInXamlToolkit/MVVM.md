
#  導入 套件
CommunityToolkit.Mvvm

# ViewModel 繼承ObservableObject
```cs

 partial class MainViewModel : ObservableObject
 {

     private Random random = new Random();


     [ObservableProperty]
     private ObservableCollection<MainModel> _dataList = new  
     
     ObservableCollection<MainModel>();

```

# view
```xml
<Window
    x:Class="DataTemplate_test1.DataTemp_Window"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:local="clr-namespace:DataTemplate_test1"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:sys="clr-namespace:System;assembly=mscorlib"
    xmlns:vm="clr-namespace:DataTemplate_test1.ViewModel"
    Title="DataTemp_Window"
    Width="800"
    Height="450"
    mc:Ignorable="d">

    <Window.DataContext>
        <vm:MainViewModel />
    </Window.DataContext>

    <Grid>
        <ListBox />

    </Grid>
</Window>

```

# DataTemplate
```cs
    <Window.DataContext>
        <vm:MainViewModel />
    </Window.DataContext>
    <Grid>

        <!-- <ListView ItemsSource="{Binding DataList}"> -->
        <!--     <ListView.View> -->
        <!--         <GridView> -->
        <!--             <GridViewColumn DisplayMemberBinding="{Binding Name}" Header="名稱" /> -->
        <!--  ~1~  更多列  @1@  -->
        <!--         </GridView> -->
        <!--     </ListView.View> -->
        <!-- </ListView> -->

        <ListBox ItemsSource="{Binding DataList}">
            <ListBox.ItemTemplate>
                <DataTemplate>
                    <StackPanel Margin="5" Orientation="Horizontal">
                        <!--  水平排列，增加邊距  -->
                        <!--  顯示名稱  -->
                        <TextBlock
                            Margin="0,0,10,0"
                            VerticalAlignment="Center"
                            Text="{Binding Name}" />
                        <!--  顯示年齡，與名稱在同一行  -->
                        <TextBlock VerticalAlignment="Center" Text="{Binding Age, StringFormat='年齡: {0}'}" />
                    </StackPanel>
                </DataTemplate>
            </ListBox.ItemTemplate>
        </ListBox>

    </Grid>

```
