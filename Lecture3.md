# Lecture 3 Editors(Vim)

##  Official note

###  English version

[Editors (Vim) · Missing Semester (mit.edu)](https://missing.csail.mit.edu/2020/editors/)

### Chinese version

[编辑器 (Vim) · the missing semester of your cs education (missing-semester-cn.github.io)](https://missing-semester-cn.github.io/2020/editors/)

##  Express notation

`Ctrl+V`  also has the expression `^V` or `Ctrl-V` or `<C+V>` 

##  Modes in Vim

1. `normal mode` navigating around a file, reading things, going from file to file and so on. Keys will not put into the buffer but used for things like navigation or making edits.
2. `insert mode` type in text. Keys will be put into the buffer.
3. `replace mode` overwrite text.

4. `visual mode` and `visual line mode` and `visual block mode` 
5. `command line mode`

##  How to change modes?

In the `normal mode`, press `i` to change to the `insert mode`, press `r` to change to the `replace mode`, press `v` to change to the `visual mode` , press `S-V` to change to the `visual line mode` , press `C-V` to change to the `visual block mode` , press` :` to change to the `command line mode` . All the other mode can press `Esc` to get back to the `normal mode`.

##  Open Vim

press `vim` in the terminal to open the Vim or `vim filaname` to open a certain file.

##  Functions in each mode

### noraml mode

1. `x`  delete the letter.
1. `i`  change to the insert mode
1. `o`  create a new line under and change to the insert mode.
1. `O`  create a new line above and change to the insert mode.
1. `r`  change th the replace mode
1. `:`  change to the command line mode
1. `h`  moves left. 
1. `j`  moves down.
1. `k`  moves up.
1. `l`  moves right.
1. `w`  moves the cursor forward by one word.
1. `b`  moves the cursor backward by one word.
1. `e`  moves the cursor to the end of the word.
1. `0`  moves to the beginning of the line.
1. `$`  moves to the end of the line.
1. `v`  change to visual mode.
1. `V`  change to visual line mode.
1. `^v` change to visual block mode.
1. `^`  moves to the first non-empty character on a line.
1. `^u` page up.
1. `^d` page down.
1. `G`   moves all the way down.
1. `gg` moves all the way up.
1. `L`   moves the cursor to the lowest line that's shown on the screen.
1. `H`   moves the cursor to the highest line that's shown on the screen.
1. `M`   moves the cursor to the middle line that's shown on the screen.
1. `f+letter` find the first letter you want in the line.
1. `F+letter` find the first letter you want before the cursor.
1. `t+letter` find the letter and move before it in the line.
1. `T+letter` find the letter before the cursor and  move **after** it in the line.
1. `\+letter` find the first leeter you want in the file.
1. `d+movement command`  delete.
1. `u`    undo.
1. `c+movement command`  delete and change to the insert mode.
1. `dd`  delete the line.
1. `cc`  delete the line and change to the insert mode.
1. `r`    replace the character with other character.
1. `^r`  redo.
1. `y+movement command`  copy.
1. `yy`  copy the whole line.
1. `p+movement command`  paste.
1. `pp`  paste the whole line.
1. `~`    change the case of characters.
1. `number+command` do the command for times.
1. `%`   jump back and forth between matching parentheses.
1. `.`   repeat the previous editing command that was made.

#### modifier command

1. `a`  around, include the paretheses itself.
2. `i`  inside, without the paretheses itself.

###  Insert mode

Just like narmal text editors. 

###  command line mode

1. `quit`                                                                                                                                                                                                                                 or `q` quit Vim, or we can say close the current window.
2. `qa` quit all the open windows.
3. `w` write and save.
4. `help key/command` get the help of the keystroke or the command.
5. `sp` create two different windows.

### Visual mode

Use movement command to select a block of text. Press `y` to copy to the paste buffer and change back to normal mode. Press `p` to paste the text.

###  Visual line mode

Use movement command to select lines of text. Other commands are like visual mode.

###  Visual block mode

Use movement command to select rectangular blocks of text. Other commands are like visual mode.

##  tab window and buffer

Vim has the idea of - there are a number of tabs, and each tab has some number of windows, and then each window has coeersponds to some buffer. But, a particular buffer can be open in zero or more windows at a time.

----------------------------

