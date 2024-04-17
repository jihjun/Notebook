
1.app.manifest文件的添加和配置，先把注释解除在改

```cs
<application xmlns="urn:schemas-microsoft-com:asm.v3">
    <windowsSettings>
      <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV1</dpiAwareness>
      <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
    </windowsSettings>
  </application>
```

2.修改App.config
```cs
<runtime>
 <AppContextSwitchOverrides value = "Switch.System.Windows.DoNotScaleForDpiChanges=true"/>
	</runtime>
```

3.xaml和cs
```cs
 <Window.LayoutTransform >
        <ScaleTransform x:Name="ScaleDPI" ScaleX="1" ScaleY="1" ></ScaleTransform>
    </Window.LayoutTransform>
``` 




```cs
  var dpi = System.Windows.Media.VisualTreeHelper.GetDpi(this);
            ScaleDPI.ScaleX = ScaleDPI.ScaleY = 1 - (dpi.PixelsPerDip - 1);
```
  
本文是简洁介绍，如需详细资料，参考吕毅大佬


[Windows 下的高 DPI 应用开发（UWP / WPF / Windows Forms / Win32） - walterlv](https://blog.walterlv.com/post/windows-high-dpi-development.html "Windows 下的高 DPI 应用开发（UWP / WPF / Windows Forms / Win32） - walterlv")

[支持 Windows 10 最新 PerMonitorV2 特性的 WPF 多屏高 DPI 应用开发 - walterlv](https://blog.walterlv.com/post/windows-high-dpi-development-for-wpf.html "支持 Windows 10 最新 PerMonitorV2 特性的 WPF 多屏高 DPI 应用开发 - walterlv")


原文链接：https://blog.csdn.net/weixin_38110122/article/details/125329464