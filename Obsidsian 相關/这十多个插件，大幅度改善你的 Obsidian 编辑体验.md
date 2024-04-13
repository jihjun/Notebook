
### **Matrix 首页推荐**

[Matrix](https://sspai.com/matrix) 是少数派的写作社区，我们主张分享真实的产品体验，有实用价值的经验与思考。我们会不定期挑选 Matrix 最优质的文章，展示来自用户的最真实的体验和观点。

文章代表作者个人观点，少数派仅对标题和排版略作修改。

---

对于不习惯 Markdown 和拆分视图的用户而言，笔记应用 Obsidian 1的编辑体验并不好：同一视图中所见并非所得，表格和 Front Matter 中无意义的标记太多，列表操作不符合直觉……为了改进 Obsidian，一些用户会选择诸如 Typora 的第三方编辑器，但这样又会失去 Wiki 链接这个核心功能。

鱼与熊掌，不可兼得吗？并非如此。得益于 Obsidian 的可扩展性，合适的主题及插件能大大提升我们在 Obsidian 码字的体验，配以合适的快捷键，使用 Obsidian 写文章，做笔记可以变得更有效率，更幸福。

本文所推荐的插件或主题都能在 Obsidian 插件市场根据扩展名称找到，读者可以根据自己的需求搜索下载。一些扩展可能不适用于移动端。

## 对阅读友好的主题和配套插件

屏幕中如果挤满了间距相同，大小近似，颜色一致的文字，显然更容易让人视觉疲劳。眼睛找不到一个用以定位的锚点，只要移开视角一小会，我们就要花功夫找「刚刚写到了/读到了哪」。很不幸，Obsidian 默认的主题恰恰是这样。

我们可以考虑换一个对眼睛友好的主题，它们可以是拉大了文字间距，也可以是将标题、列表渲染成彩色（像默认主题那样更改字重差异并不明显）。这会对阅读及编辑有不小的帮助。例如 California Coast、Dracula、Discordian。

![](https://cdn.sspai.com/editor/u_/c4f8tr5b34tb094k5bf0.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

图片来自：Dracula GitHub 项目说明

一些用户不知道的是，很多这样的主题还有配套的插件，里面还有不少默认不开启的隐藏功能。例如 California Coast 主题有配套插件 Style Settings; Discordian 有配套插件 Discordian Theme。我们可以自定义标题的大小、列表的渲染形式等等。

![](https://cdn.sspai.com/editor/u_/c4f8trdb34tb0oe10320.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

## 所见即所得：Ozan's Image in Editor Plugin

Markdown 为人称道的一点就是一边写内容，一些进行简单排版。而比起传统的编辑 - 预览两栏模式，Typora 这样的实时渲染模式更受欢迎。Ozan's Image in Editor Plugin 插件则能在 Obsidian 中实现所见即所得。

安装并启用该插件后，在设置 > 插件选项 > Ozan's Image in Editor Plugin 界面下，勾选「Render Toggle」、「WYSIWYG Like Experice」两项，然后重启 Obsidian 就能看到效果。下图左侧是编辑模式，右侧是预览模式，装了扩展后两者基本没有区别。

![](https://cdn.sspai.com/editor/u_/c4f8trlb34tb0oe1032g.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

插件还提供了渲染 Ifame、PDF 等选项，可按需开启。「Refresh Images after Changes」建议不要勾选，它会导致图片预览异常2。

该插件目前有两个小问题。其一，当图片语法中含有数字时（如`![123Mirtle](kepa.png)`），不会显示预览。其二，编辑模式选中文本有时候会错位。

## 输入时间更自然：Natural Language Dates

无论是日记、周志，还是在待办中，时间都是出现频率极高的那一类词汇。而在输入时间时，「x 年 x 月 x 日」这样表述像机械般生硬，「今天」、「明天」或「下周一」这样的说法才显得自然。因此，很多待办应用都支持了自然语言键入。Natural Language Dates 为 Obsidian 带来了这一特性。

它主要有三个功能：设置后使用快捷键迅速输入时间、日期；键入「`@` + 自然语言」自动转换为日期；将现有的自然语言通过快捷键或动作（Action）转换为日期。

![](https://cdn.sspai.com/editor/u_/c4f8trtb34tb094k5bfg.gif)

图源：项目官网

插件默认为英文，如果想要使用中文的话，可以自己修改文件夹中的 JS 的一些关键词，还算简单。

## 改善超链接：Paste URL into Selection 和 Auto Link Title

Paste URL into Selection 做的事情非常简单。假设我们剪贴板首位为链接「[https://example.com](https://sspai.com/link?target=https%3A%2F%2Fexample.com%2F)」，此时选中一段文字「TEXT」，粘贴，这个链接就会和文字结合在一起成为「`[TEXT](https://example.com)`」。

![](https://cdn.sspai.com/editor/u_/c4f8tsdb34tb0b5qhejg.gif)

图源：项目官网

该插件的设置界面有一个条目控制当没有文字选中时粘贴链接的行为，如果我们使用了自动格式化图片链接的软件（如 ShareX，Picgo autocopy），建议选择「Do Nothing」。

Auto Link Title 则会在粘贴链接的时候自动抓取网页的标题，填充为文字。

![](https://cdn.sspai.com/editor/u_/c4f8tslb34tb094k5bg0.gif)

图源：项目官网

可惜的是，目前两个插件并不兼容。在选中文字粘贴链接时，Auto Link Title 会让 Paste URL into Selection 失去作用。

## 让列表更智能：Outliner

在没有装 Outliner 之前，Obsidian 中的有序和无序列表在编辑上太过自由，我们甚至可以在一级有序列表后插入一个三级或四级无序列表；而在另一些方面过于死板，比方说只有全选一个条目，才能用 Tab 调整它的层级。Outliner 能让 Obsidian 摇身变为简单方便的大纲工具。

首先，它严格限制了列表的层级：一级列表后只能插入二级列表。其次，它让列表的编辑更方便：只需光标在一个列表条目，就能使用 Tab 对它进行操作，Ctrl + A 此时也不会再选中全文，而是一个小条目。此外，Outliner 还给列表的编辑界面添加了类似代码编辑器那样的层级样式，让它们看起来更有条理。

![](https://cdn.sspai.com/editor/u_/c4f8tstb34tb0h9pknng.gif)

图源：项目官网

如果想要使用此扩展，强烈建议你为以下五个操作添加顺手的快捷键：

1. 有序/无序列表：用于切换列表样式。
2. 折叠/展开当前行：用于展开折叠列表；需要在设置中打开「折叠缩进」选项。
3. 段落的上移/下移：用于快速调整列表条目的位置。Outline 插件设置中有相似的动作，但建议直接使用 Obsidian 本体提供的快捷方式，适用范围更广。

## 让 Wiki 链接方便：Note Refactor

默认情况下，使用双中括号可以唤出 Obsidian 的标题建议界面，并创建 Wiki 链接。但如果我们想要为已经写过的文字创建 Wiki 链接呢？这时候我们可以使用 Note Refactor 插件。

我们只需要选中一段文字，然后按下设置的快捷键，就能以这段文字为文件名创建新笔记，并在新的面板打开。如果笔记已经存在，会打开这个存在的笔记。

![](https://cdn.sspai.com/editor/u_/c4f8tstb34tb0h9pkno0.gif)

图源：项目官网

此插件还能够实现根据标题将一个大文档拆分的效果，拆分后的小文档会在原文档留下 Wiki 链接。

## 让插入表格更简单：Advanced Tables

Advanced Tables 插件位列下载排行榜第二不是没有道理的：Markdown 的表格语法比在 Word 中插入表格还要麻烦，一旦想要增改某个内容，更是灾难。该插件则大大简化了表格输入的流程。

只需要输入「`|` + 作为标题的文字」，再按下 Tab，就会触发插件的自动补全语法。之后通过 Tab/Shift + Tab 来在表格之间移动光标，通过 Enter 完成输入，不多时一个好看的表格就写完了。

![](https://cdn.sspai.com/editor/u_/c4f8tt5b34tb0b5qhek0.gif)

图源：项目官网

同时，像上图展示的那样，它还有浮动的工具栏，提供了快捷键之外的表格编辑选择。

## 代码高亮：Editor Syntax Highlight

无需多言，只要在 Obsidian 中记录代码的需求，Editor Syntax Highlight 就是必装插件。它能让编辑界面的代码，页面的元数据都有高亮效果。配合前面提到的 Ozan's Image in Editor Plugin，便能让编辑界面胜似预览界面。

![](https://cdn.sspai.com/editor/u_/c4f8ttdb34tb0b5qhekg.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

图源：项目官网

## **让 Markdown 更统一：Markdown Prettifier**

虽说都叫 Markdown，但实际上标记和格式五花八门，很多编辑器也就一概不问，全都兼容。这导致一些用户就习惯于混用各类标记。千奇百怪的格式既不美观，在某些编辑器上也不能正确渲染。规范自己的书写习惯是长期的过程，使用软件辅助统一 Markdown 格式则来得更简单。

我习惯把 Markdown Prettifier 的快捷键设置成 Ctrl + S，按下此快捷键后，插件会按预先的配置将文章的 Markdown 格式规范和统一。比如将所有的无序列表标记统一置换为`-`。

![](https://cdn.sspai.com/editor/u_/c4f8ttlb34tb0oe10330.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

Markdown Prettifier 还有一个非常好用的功能：更新文件的元数据。勾选插件设置中的「Update header」和「Add new headers」，在 header 设置中填入模板，例如`updated:'{{date:YYYY-MM-DD HH:mm:ss}}'`，它会在按下快捷键后更新笔记中的`updated`一栏的数据为当前时间，如果没有相关的条目，则会自动补充。

## **快捷键设置很重要**

当然，想要在编辑的时候不手忙脚乱，还得依靠方便好记的快捷键。但每个人的系统环境不同，个人习惯也不同，方便与否还得靠自己尝试。这里展示我自己在 Windows 下的快捷键绑定原则和大部分快捷键列表，供大家参考。

- Ctrl 键可以和几乎所有键绑定，不与其他软件设置冲突即可。
- Shift 键一般不作为修饰键，因为会和输入冲突。
- Ctrl + Shift 键是 CTRL 的增强功能或者对应功能。
- Alt 键尽量不和字母键绑定。在 Ctrl 键被占用的情况下使用。

![](https://cdn.sspai.com/2021/08/20/78999fd467aa0a748781630cf9ee7a06.png?imageView2/2/w/1120/q/90/interlace/1/ignore-error/1/format/webp)

## **总结**

推荐了这么多插件，其实只是想让 Obsidian 的编辑体验更好一点而已。但这是 Obsidian 的问题吗？更深层次的原因应该是 Markdown：虽然它期望将编辑和排版完美结合，但结果，既不能方便地搞定复杂多变的排版；也将一部分文字内容的编辑复杂化，输入的无意义标记比正文还多。放眼望去，几乎所有支持 Markdown 主流笔记软件都要考虑如何优化自己的输入体验，或者像 Notion 那样用各类关键词来引导创建格式，或者像本文一样采用各种快捷键，或者兼而有之。

如果是这样的话，Markdown 为何还有如此多的人使用？它确实简化了诸如标题，序号等格式的输入方式，但一些富文本编辑器目前也能做到。我想，最重要的是，Markdown 作为文本文件所独具的通用性。不如换一种问法：为什么用 Obsidian 记录？—— 它既不会像 Evernote 搞一套别的软件都识别不了的格式，也不会像 Notion 那样将文件名称搞的乱七八糟。在 Obsidian 上编辑的，同样能在 GitHub，VScode 和 Markor 上正常打开和渲染。这自然归功于简单，通用的 Markdown。

那么，为什么不使用其他本地 Markdown 笔记呢？因为 Obsidian 给笔记间搭建了联系的桥梁，并有着强大丰富的扩展体系，正如本文展示的那样。