VSCode 版本: 1.45.1

设置 Google format 方式：
https://www.cnblogs.com/zzoo/p/vscode_cpp_extensions.html

其中：Clang_format_style 决定格式化形式，若为file，则调用在workspace中的.clang-format
clang-format工具使用.clang-format来实现自定义格式化，使用" clang-format -style=可选格式名 -dump-config > .clang-format "导出clang-format，其中可选格式名为：LLVM、Google等。若要在团队中统一格式，则配置此文件即可。
C_Cpp: Clang_format_fallback Style选项则是在 Clang_format_style=file失效（即找不到.clang-format文件时）的应对方案。参考截图中可以配置的value。
在这里我的方案是，Clang_format_style=file，以应对以后团队中为统一格式而生成.clang-format文件。目前没有此文件，所以走Clang_format_fallback Style选项中的{key:value...}，即{ BasedOnStyle: Google, ColumnLimit: 120}//这里我只有列最大120的需求（ps，vscode编辑器中显示列标尺的设置：“editor.rulers:[120]”）
最后，在代码编辑器中，右键-使用...格式化文档-配置默认格式化程序，改为c/c++插件即可。
