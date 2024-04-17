在之前写的这篇文章 [WPF: 只读依赖属性的介绍与实践](http://www.cnblogs.com/wpinfo/p/readonly_dp.html) 中，我们介绍了在 WPF 自定义控件中如何添加只读依赖属性，并且使其结合属性触发器 (Trigger) 来实现对控件样式的改变。事实上，关于触发器，在 WPF 中除了属性触发器，还有事件触发器 (EventTrigger) 和数据触发器 (DataTrigger)。此外，为了控制控件外观的切换，除了可以使用触发器外，我们还可以使用 VisualStates 和 VisualStateManager 来完成。

本文接下来会分别简单地介绍 Triggers 和 VisualStateManager，并且介绍一下它们的区别；了解这些，将会有助于我们在开发自定义控件时，能够明白何时选择属发器，何时选择 VisualStateManager。

[回到顶部](https://www.cnblogs.com/wpinfo/p/wpf_triggers_vsm.html#_labelTop)

## 一、触发器 (Triggers)

前面说过，触发器有三类，它们分别是：

- 属性触发器 (Trigger/MultiTrigger)
- 事件触发器 (EventTrigger)
- 数据触发器 (DataTrigger/MultiDataTrigger)

这些触发器的主要作用是：当指定属性的值发生改变或者事件触发时来更改控件的外观或行为；而且被触发器监测的属性必须是依赖属性，事件必须是路由事件；它们能够用在这些地方：DataTemplate, ControlTemplate, Style, 以及控件的 Inline 属性中。

### 1、属性触发器

特点：

1. UIElement 属性改变时，执行 Trigger；
2. 必须设置 Property 和 Value；必须包括 Setter 对象（不支持 EventSetter）；
3. 可以设置 EnterActions 和 ExitActions；
4. 当条件不符合时，会将属性值还原到原来的值 ；

例如：


``` cs
<Style x:Key="ButtonStyle" TargetType="{x:Type Button}">
    <Style.Triggers>
        <Trigger Property="IsPressed" Value="True">
            <Setter Property="Opacity" Value="0.5" />
        </Trigger>
        <Trigger Property="IsEnabled" Value="False">
            <Setter Property="Foreground" Value="Red" />
        </Trigger>
    </Style.Triggers>
</Style>

```



**MultiTrigger**

特点：元素 (UIElement) 或控件的多个属性改变时，执行 Trigger，如：

![复制代码](https://assets.cnblogs.com/images/copycode.gif)

```cs
<Style x:Key="MulitTriggerButtonStyle" TargetType="Button">
    <Style.Triggers>
        <MultiTrigger>
            <MultiTrigger.Conditions>
                <Condition Property="IsPressed" Value="True" />
                <Condition Property="Background" Value="BlanchedAlmond" />
            </MultiTrigger.Conditions>
            <MultiTrigger.Setters>
                <Setter Property="Foreground" Value="Blue" />
                <Setter Property="BorderThickness" Value="5" />
                <Setter Property="BorderBrush" Value="Blue" />
            </MultiTrigger.Setters>
        </MultiTrigger>
    </Style.Triggers>
</Style>
```

### 2. 事件触发器

特点：

1. 当 FrameworkElement 的路由事件(RouteEvent)触发时，执行 EventTrigger；
2. 通常用来执行一些动画；
3. 不会还原成原来的值（不像 Property Trigger 一样）；

例如：

![复制代码](https://assets.cnblogs.com/images/copycode.gif)
```cs

<Border  …>
    <Border.Triggers>
        <EventTrigger RoutedEvent="Mouse.MouseEnter">
            <BeginStoryboard>
               <Storyboard>
                <ColorAnimation Duration="0:0:1" Storyboard.TargetName="MyBorder" 
                                Storyboard.TargetProperty="Color" To="Gray" />
               </Storyboard>
            </BeginStoryboard>
        </EventTrigger>
    </Border.Triggers>
 </Border>
```



### 3. 数据触发器

特点：数据绑定源的值符合 Trigger 指定的条件时，执行 Trigger；例如：


```cs
    <DataTemplate.Triggers>
       <DataTrigger Binding="{Binding Path=Picture}" Value="{x:Null}">
        <Setter TargetName="viewImage" Property="Source" Value="/Images/noImage.png" />
        ……
       </DataTrigger>
    </DataTemplate.Triggers>
```
    


**MultiDataTrigger**

特点：当多个绑定值符合 Trigger 的 Conditions 指定的条件时，执行 Trigger；例如：


```cs

<DataTemplate.Triggers>
    <MultiDataTrigger>
        <MultiDataTrigger.Conditions>
            <Condition Binding="{Binding Path=Picture}" Value="{x:Null}" />
            <Condition Binding="{Binding Path=Title}" Value="Waterfall" />
        </MultiDataTrigger.Conditions>
        <MultiDataTrigger.Setters>
           <Setter TargetName="viewImage" Property="Source" Value="/Images/noImage.png"/>
           <Setter TargetName="viewImage" Property="Opacity" Value="0.5" />
           <Setter TargetName="viewText" Property="Background" Value="Brown" />
        </MultiDataTrigger.Setters>
    </MultiDataTrigger>
</DataTemplate.Triggers> 
```


最后，要说明的是，对于控件的内联 (Inline) 属性，仅能设置 EventTrigger，如：


```cs
<Button>
    <Button.Triggers>
        <EventTrigger …>
        </EventTrigger>
    </Button.Triggers>
</Button>

```

所以，总结来看，以上三类触发器的使用场景就是当控件的属性值被更改或者事件被触发时，更修改控件的外观或行为；并且属性触发器有独特的特点，就是在属性值还原时，样式也会随即恢复。

理解了这些，在开发自定义控件时，我们就可以通过为自定义控件合理地定义依赖属性、只读依赖属性以及路由事件，来控制控件的显示。接下来，我们来看 VisualStateManager，它也可以解决同样的问题。


## 二、VisualStateManager

要使用 VisualStateManager，需要定义 VisualState；在 VisualState 中定义控件的不同的状态以及每种状态下的样式，然后，在代码中合适的地方，我们可以使用 VisusalStateManager 类的 GoToState 来切换到对应的状态，从而实现样式的切换。

所以，总括地说，这里涉及了以下四个方面：

1. VisualState: 视图状态(Visual States)表示控件在一个特殊的逻辑状态下的样式、外观；
2. VisualStateGroup: 状态组由相互排斥的状态组成，状态组与状态组并不互斥；
3. VisualTransition: 视图转变 (Visual Transitions) 代表控件从一个视图状态向另一个状态转换时的过渡；
4. VisualStateManager: 由它负责在代码中来切换到不同的状态；

每个 VisualState 都属于一个状态组 (VisualStateGroup)，也即一个 VisualStateGroup 中可以定义多个 VisualState；并且，我们也可以定义多个 VisualStateGroup；需要再次强调的是：同一个 VisualStateGroup 中 VisualState 是互斥的，而不同的 VisualStateGroup 中的 VisualState 是在同一时刻是可以共存的。以 Button 为例：

![](https://images2017.cnblogs.com/blog/676860/201802/676860-20180214150414187-463687289.png)

我们看到，在它里面，定义了三个 VisualStateGroup，分别是 CommonStates（正常状态）、FocusStates（焦点状态）、ValidationStates（验证状态），而每个 VisualStateGroup 下又有若干个 VisualState。在 CommonStates 中，按钮可以是 Normal 、MouseOver 或 Pressed（只能是三者之一），但它却可以结合其它 VisualStateGroup 中的 VisualState 来显示，如按钮具有焦点时且鼠标移动到其上，这就结合了 MouseOver 与 Focused 两种状态。以下它的部分代码：


```cs

<ControlTemplate TargetType="Button">
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="CommonStates">
                <VisualStateGroup.Transitions>
                    <VisualTransition GeneratedDuration="0:0:1" To="MouseOver" />
                    <VisualTransition GeneratedDuration="0:0:1" To="Pressed" />
                    <VisualTransition GeneratedDuration="0:0:1" To="Normal" />
                </VisualStateGroup.Transitions>
                <VisualState x:Name="Normal" />
                <VisualState x:Name="MouseOver">
                    <Storyboard>
                        <ColorAnimation
                            Storyboard.TargetName="BackgroundBorder"
                            Storyboard.TargetProperty="Background.(SolidColorBrush.Color)"
                            To="#A1D6FC"
                            Duration="0" />
                    </Storyboard>
                </VisualState>
                <VisualState x:Name="Pressed">
                    <Storyboard>
                        <ColorAnimation
                            Storyboard.TargetName="BackgroundBorder"
                            Storyboard.TargetProperty="Background.(SolidColorBrush.Color)"
                            To="#FCA1A1"
                            Duration="0" />
                    </Storyboard>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        <Border
            x:Name="BackgroundBorder"
            Background="{TemplateBinding Background}"
            BorderBrush="{TemplateBinding BorderBrush}"
            BorderThickness="{TemplateBinding BorderThickness}"
            SnapsToDevicePixels="true" />
        <ContentPresenter
            Margin="{TemplateBinding Padding}"
            HorizontalAlignment="{TemplateBinding HorizontalContentAlignment}"
            VerticalAlignment="{TemplateBinding VerticalContentAlignment}"
            Focusable="False"
            RecognizesAccessKey="True"
            SnapsToDevicePixels="{TemplateBinding SnapsToDevicePixels}" />
    </Grid>
</ControlTemplate>

```

在自定义控件的开发过程中，我们也可以采用同样的原则，即在 XAML 中定义 VisualStateGroup、VisualState 以及 VisualTransition（可选），由借助于 VisualStateManager 来实现切换。以下是一个带水印功能的 TextBox 中 VisualStates 的定义：


```cs
<VisualStateManager.VisualStateGroups>
   <VisualStateGroup x:Name="WatermarkGroup">
       <VisualStateGroup.Transitions>
           <VisualTransition From="ShowWatermarkState" To="HideWatermarkState">
              <Storyboard>
                 <DoubleAnimation Storyboard.TargetName="PART_Watermark"
                                  Storyboard.TargetProperty="Opacity" From="1"
                                  To="0" Duration="0:0:2" />
               </Storyboard>                                        
            </VisualTransition>
            <VisualTransition From="HideWatermarkState" To="ShowWatermarkState">
               <Storyboard>
                  <DoubleAnimation Storyboard.TargetName="PART_Watermark"
                                   Storyboard.TargetProperty="Opacity" From="0"
                                   To="1" Duration="0:0:2" />
               </Storyboard>
            </VisualTransition>
         </VisualStateGroup.Transitions>
         <VisualState x:Name="ShowWatermarkState">
            <Storyboard>
               <ObjectAnimationUsingKeyFrames Duration="0:0:0"
                             Storyboard.TargetName="PART_Watermark"
                             Storyboard.TargetProperty="(UIElement.Visibility)">
                  <DiscreteObjectKeyFrame KeyTime="0:0:0"
                             Value="{x:Static Visibility.Visible}"/>
               </ObjectAnimationUsingKeyFrames>
            </Storyboard>
         </VisualState>
         <VisualState x:Name="HideWatermarkState">
            <Storyboard>
               <ObjectAnimationUsingKeyFrames Duration="0:0:0"
                             Storyboard.TargetName="PART_Watermark"
                             Storyboard.TargetProperty="(UIElement.Visibility)">
                   <DiscreteObjectKeyFrame KeyTime="0:0:0"
                             Value="{x:Static Visibility.Collapsed}"/>
               </ObjectAnimationUsingKeyFrames>
            </Storyboard>
        </VisualState>                                
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```


可以看出，它定义了 WatermarkGroup 组，并在其中定义了 ShowWatermarkState 和 HideWatermarkState 两个 VisualState，并且，通过定义的 VisualTransition 能够实现在这两种状态下切换时，水印文本会有淡入、淡出的效果。

最后，在代码中，调用  VisualStateManager.GoToState 方法来切换到合适的状态即可：


```cs

private void UpdateState()
{
   bool textExists = Text.Length > 0;
   var watermark = GetTemplateChild("PART_Watermark") as FrameworkElement;
   var state = textExists || IsFocused ? "HideWatermarkState" : "ShowWatermarkState";
 
   VisualStateManager.GoToState(this, state, true);
}
 
protected override void OnGotFocus(RoutedEventArgs e)
{
   base.OnGotFocus(e);
   UpdateState();
}
 
protected override void OnLostFocus(RoutedEventArgs e)
{
   base.OnLostFocus(e);
   UpdateState();
}

```


最后，何时切换状态呢？一般情况下，包括但不限于以下几个场合或地方：

- 在 OnApplyTemplate 方法（即应用模板时）；
- 当某个属性改变时（可以在 PropertyChangedCallback 中调用）；
- 当某个事件发生后； 


## 三、Triggers 与 VisualStateManager 的区别

以上我们看到 Trigger 和 VisualStateManager 都可以完成相同的事，不过它们也是有区别的，主要有以下几点：

1. Triggers 仅仅在 XAML 中使用（尽管它可能要用到我们的自定义属性或事件，但对于更改状态这件事来说，我们只要在 XAML 中操作即可），而 VisualStateManager 则 XAML 和 C# 代码都需要；
2. 对于 Trigger，定义模板的人可以自由地指定当条件符合时时该有何种的变化；而对于 VisualStateManager，则需要控件开发人员定义不同的 VisualState，然后定义模板的人根据约定（根据 [TemplateVisualStateAttribute](https://msdn.microsoft.com/en-us/library/system.windows.templatevisualstateattribute(v=vs.110).aspx) 特性）来定义在控件不同状态下的样式；
3. 对于 Trigger，它是对事件、属性或者所绑定的数据发生变化时，作出对应的改变；而 VSM 则可以自由控制状态的切换，并定在切换时，还可以指定 VisualTransition；
4. 最后， VisualStateManager 不仅支持 WPF，而且也支持 UWP，我们可以说它是“跨平台”的，而 Trigger 在 UWP 上不被支持。



## 总结

本文主要讨论了 WPF 中的触发器 Triggers 和 VisualStateManager，在设计自定义控件时，它们都能够辅以完成控件样式切换的功能；我们分别介绍了它们二者的使用方法以及使用要点；最后我们也总结了它们的区别。当我们了解了它们各自的特点以及它们的区别后，我们就可以有选择地、更方便地来设计我们的自定义控件，并在 WPF 开发过程中自由地运用它们。

参考资料：

_[VisualStateManager and alternative to Triggers](http://putridparrot.com/blog/visualstatemanager-and-alternative-to-triggers/)_

https://www.cnblogs.com/wpinfo/p/wpf_triggers_vsm.html