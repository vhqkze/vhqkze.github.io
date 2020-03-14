# Linux安装配置rime输入法

## 安装
安装ibus-rime
```bash
sudo dnf install ibus-rime
```
重启ibus
```bash
ibus restart
```
进入设置，进入“区域与语言”设置，点击“输入源”下面的加号，在“添加输入源”中选择“汉语（中国）”，然后选则“中文（Rime）”，这样右上角的输入法托盘里面就可以切换输入法了。

按组合键`` Ctrl+` ``或`F4`键唤出输入方案选单，由此调整Rime输入法最常用的选项。

## 定制rime
和许多软件一样，Rime 的配置也是分「共享/系统」和「用户」的。比如对我的 Fedora 里的 ibus-rime 来说，系统配置位于 `/usr/share/rime-data` 目录下，而用户配置位于 `~/.config/ibus/rime` 目录下，后者的优先级高于前者。此外 ibus-rime 还会在启动时执行一次「部署」，根据原始配置和补丁文件，编译一份合并之后的配置（以及编译出来的二进制词库），放在 `~/.config/ibus/rime/build` 目录里。我想看看自己的补丁并入原文件之后最终的生效配置，就看这里面的文件。

如果你改动了任意配置文件，也需要执行一次「部署」，让 Rime 重新编译新的配置并应用。

Rime 的主要配置文件有这么几个：

- default.yaml —— 各方案共享的全局配置
- default.custom.yaml —— （可选）对 default.yaml 的「补丁」
- XXX.schema.yaml —— XXX 输入方案的配置
- XXX.custom.yaml —— （可选）对 XXX.schema.yaml 的「补丁」
- XXX.dict.yaml —— XXX 输入方案的词库（字典）

进入用户配置目录，创建`default.custom.yaml`文件，里面写上以下内容：
```yaml
# default.custom.yaml
# save it to: 
#   ~/.config/ibus/rime  (linux)
#   ~/Library/Rime       (macos)
#   %APPDATA%\Rime       (windows)

patch:
  schema_list:
    - schema: luna_pinyin
    - schema: double_pinyin_flypy

```

创建`double_pinyin_flypy.custom.yaml`文件，里面写上以下内容：
```yaml
# double_pinyin_flypy.custom.yaml

patch:
  schema:
    name: 小鹤双拼
  punctuator:
    import_preset: symbols
    half_shape:
      "#": ["#"]
  recognizer:
    patterns:
      punct: "^/([a-z]+|[0-9])$"
```



## 参考
- [Rime 必知必会](https://github.com/rime/home/wiki/RimeWithSchemata)
- [Rime 下载及安装](https://rime.im/download/#linux)
- [Rime 定制指南](https://github.com/rime/home/wiki/CustomizationGuide)
- [schema详解](https://github.com/LEOYoon-Tsaw/Rime_collections/blob/master/Rime_description.md)
