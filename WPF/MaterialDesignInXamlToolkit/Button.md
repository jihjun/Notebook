這個資源字典定義了一系列 Material Design 風格的 WPF 按鈕樣式。每種樣式透過不同的背景、前景色、邊框厚度、文字風格等設置，為按鈕提供獨特的視覺效果。以下是每個樣式的詳細說明：

### 一般按鈕樣式
- **`MaterialDesignRaisedButton`**: 標準提升按鈕樣式，具有中等亮度的背景和前景色。
- **`MaterialDesignFlatButton`**: 平面按鈕樣式，背景透明，只顯示文字和/或圖標。
- **`MaterialDesignIconButton`**: 圖標按鈕樣式，專為僅含圖標的按鈕設計，無文字內容。
- **`MaterialDesignOutlinedButton`**: 輪廓按鈕樣式，具有透明背景和細邊框。
- **`MaterialDesignFloatingActionMiniButton`和`MaterialDesignFloatingActionButton`**: 浮動動作按鈕，用於表示主要動作，分為迷你大小和標準大小兩種。
- **`MaterialDesignToolButton`**: 工具按鈕樣式，用於工具欄中的按鈕，通常比標準按鈕小。

### 亮/暗色變體
大多數基本樣式都有亮色和暗色的變體，例如：
- **`MaterialDesignRaisedLightButton`和`MaterialDesignRaisedDarkButton`**: 提升按鈕的亮色和暗色變體。
- **`MaterialDesignFlatLightButton`和`MaterialDesignFlatDarkButton`**: 平面按鈕的亮色和暗色變體。

### 次要色系列
同樣，許多樣式也提供了次要色彩的選項，如`Secondary`、`SecondaryLight`和`SecondaryDark`後綴的樣式。這些用於在UI中創建層次和強調。

### 背景色系列
部分樣式使用`Bg`後綴，意味著這些樣式設計了特定的背景色，通常用於需要在背景色和按鈕色之間有明顯區別的場合。

### 特殊樣式
- **`MaterialDesignPaperButton`**: 這是一種特殊樣式，模仿紙張效果，有著較淺的背景色和細微的陰影效果，用於需要突出的地方。
- **`MaterialDesignIconForegroundButton`**: 專為那些需要根據所在容器調整前景色的圖標按鈕設計。

每種樣式都可以透過`x:Key`在XAML中直接引用，例如，要應用標準提升按鈕樣式，可以這樣寫：

```xml
<Button Style="{StaticResource MaterialDesignRaisedButton}" Content="Click Me"/>
```

這種方式使得在遵循Material Design指南的WPF應用程式開發中，能夠輕鬆地保持一致的視覺風格。