# 替换 Windows 微软雅黑字体

## 注意事项

- 替换前一定要备份原本的字体，以便恢复。
- 替换字体后，若在 Microsoft Office、WPS 等软件中编辑的内容使用了微软雅黑字体，可能会对导出图片、打印等功能产生影响。

## 系统版本

```text
Windows 11 22H2 22621.3880
Windows Feature Experience Pack 1000.22700.1020.0
```

## 使用的工具

- `ttfname3.exe`
- `UniteTTC.exe`

## 操作步骤

> 本文中以 `新字体.ttc` 和 `msyh.ttc` 为例（实际上还需要对 `msyhbd.ttc` 和 `msyhl.ttc` 进行操作）。若 `新字体` 是 `TTF`，对其可以跳过第一步中的操作。

### 一、拆分字体 TTC 为 TTF

依次执行以下命令：

```text
UniteTTC.exe 新字体.ttc
UniteTTC.exe msyh.ttc
```

或将 `TTC` 字体文件逐个拖到 `UniteTTC.exe` 上，每次操作会把对应的字体拆分为多个 `XXX.TTF` 文件。

对于 `msyh.ttc`，将会拆分为两个 `TTF` 文件，分别为 `msyh001.TTF`（微软雅黑）和 `msyh002.TTF`（Microsoft Yahei UI）。

### 二、生成字体 XML

取 `新字体` 的其中一个 `TTF` 文件（**后续还需要用到**），拖到 `ttfname3.exe` 上，生成 `新字体.xml`。

将微软雅黑的两个 TTF 文件拖到 `ttfname3.exe` 上，生成 `msyh001.xml` 和 `msyh002.xml`。

### 三、替换微软雅黑字体 XML 中的 Header 标签内容

使用文本编辑器打开 `新字体.xml` `msyh001.xml` `msyh002.xml`，复制 `新字体.xml` 的 `Header` 标签部分，粘贴到 `msyh001.xml` 和 `msyh002.xml` 中的 `Header` 标签部分进行替换。

### 四、生成修改字体信息后的新字体文件

将 `msyh001.xml` 和提取的 `新字体.TTF` 同时选中然后拖到 `ttfname3.exe` 上，生成 `msyh001_mod.TTF`。

将 `msyh002.xml` 和提取的 `新字体.TTF` 同时选中然后拖到 `ttfname3.exe` 上，生成 `msyh002_mod.TTF`。

### 五、合并字体 TTF 为 TTC

执行以下命令，将 `msyh001_mod.TTF` 和 `msyh002_mod.TTF` 合并为 `msyh_new.ttc`：

```text
UniteTTC.exe msyh_new.ttc msyh001_mod.TTF msyh002_mod.TTF
```

### 六、替换系统字体

将处理好的三个字体文件 `msyh_new.ttc` `msyhbd_new.ttc` `msyhl_new.ttc` 放到同一个**空白**的文件夹中（例如 `D:\fonts`），**后续执行命令需要用到**。

对文件进行重命名（对应系统中的字体文件名）：

```text
msyh_new.ttc -> msyh.ttc
msyhbd_new.ttc -> msyhbd.ttc
msyhl_new.ttc -> msyhl.ttc
```

按住 `Shift` 键点击 Windows 菜单中的 `重启` 选项，等待系统重启后进入“高级选项”界面，依次选择 `疑难解答 -> 高级选项 -> 命令提示符`。

在弹出的“命令提示符”窗口中执行以下命令：

```text
xcopy D:\fonts C:\Windows\Fonts
```

根据提示，输入字母 `A` 确认用新的字体文件覆盖系统中对应的字体文件。

执行 `exit` 命令，或关闭“命令提示符”窗口，然后选择“继续”以重启系统。
