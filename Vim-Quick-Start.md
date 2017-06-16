vim-quick-start.md

Vim是从 vi 发展出来的一个文本编辑器。

vi/vim 的使用

基本上 vi/vim 共分为三种模式: 分别是命令模式（Command mode），插入模式（Insert mode）和底线命令模式（Last line mode）。 

(1) 命令模式（Command mode）

用户刚刚启动 vi/vim，便进入了命令模式。
控制屏幕光标的移动，字符、字或行的删除，移动复制某区段及进入Insert mode下，或者到 last line mode。

i 切换到插入模式，以输入字符。
x 删除当前光标所在处的字符。
: 切换到底线命令模式，以在最底一行输入命令。

进入插入或取代的编辑模式:

i, I   进入插入模式(Insert mode)：
i 为『从目前光标所在处插入』
I 为『在目前所在行的第一个非空格符处开始插入』。 (常用)

a, A    进入插入模式(Insert mode)：
a 为『从目前光标所在的下一个字符处开始插入』
A 为『从光标所在行的最后一个字符处开始插入』。(常用)

o, O    进入插入模式(Insert mode)：
这是英文字母 o 的大小写。
o 为『在目前光标所在的下一行处插入新的一行』
O 为在目前光标所在处的上一行插入新的一行！(常用)

r, R    进入取代模式(Replace mode)：
r 只会取代光标所在的那一个字符一次；
R会一直取代光标所在的文字，直到按下 ESC 为止；(常用)

(2) 插入模式/编辑模式（Insert mode）

只有在Insert mode下，才可以做文字输入，按「ESC」键可回到命令行模式。

BACKSPACE，退格键，删除光标前一个字符
DELETE，删除键，删除光标后一个字符
ENTER，回车键，换行
ESC，退出输入模式，切换到命令模式
方向键，在文本中移动光标

(3) 底线命令模式（Last line mode）

将文件保存或退出vi，也可以设置编辑环境，如寻找字符串、列出行号等。

在命令模式下按下`:`（英文冒号）就进入了底线命令模式。
按`ESC`键可随时退出底线命令模式。

:q 退出程序
:w 保存文件
:wq 保存并退出
:q! 强制退出，不保存
:x  保存并退出 (同:wq)

进入vi的命令 

vi filename :打开或新建文件，并将光标置于第一行首 
vi +n filename ：打开文件，并将光标置于第n行首 
vi + filename ：打开文件，并将光标置于最后一行首 
vi +/pattern filename：打开文件，并将光标置于第一个与pattern匹配的串处 
vi -r filename ：在上次正用vi编辑时发生系统崩溃，恢复filename 
vi filename....filename ：打开多个文件，依次进行编辑 


h 或 向左箭头键(←)    光标向左移动一个字符
j 或 向下箭头键(↓)    光标向下移动一个字符
k 或 向上箭头键(↑)    光标向上移动一个字符
l 或 向右箭头键(→)    光标向右移动一个字符


:set nu 显示行号，设定之后，会在每一行的前缀显示该行的行号
:set nonu


删除所有行: `:%d`


- 跳到文本的最后一行：按字母 `G`, 即`shift+g`  
- 跳到最后一行的最后一个字符: 先即按`G`(即`shift`+`g`)，之后按`$`键(即`shift`+`4`)。
- 跳到第一行的第一个字符: 先按两次小写字母“g” 
- 跳转到当前行的第一个字符：在当前行按“0”

问: vim 如何复制多行文本？
答: 在命令模式下，将光标移动到将要复制的首行处，按`nyy`复制n行；其中n为1、2、3……


Reference:

http://www.vim.org/  
http://www.study-area.org/tips/vim/  
http://www.jianshu.com/p/bcbe916f97e1  
http://blog.jobbole.com/18339/  