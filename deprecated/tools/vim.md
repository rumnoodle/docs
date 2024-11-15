# Vim Cheats

## Modes

i       enter insert mode at cursor
I       enter insert mode at beginnning of line
a       enter insert mode after cursor
A       enter append mode at end of line
v       enter visual mode character selection
V       enter visual mode line selection
ctrl-v  enter visual mode block selection
esc     exit insert mode and enter command mode


## Files

:edit <filename>    opens existing file in new buffer


## Movement

h       move left
j       move down
k       move up
l       move right

0       beginning of line
$       end of line

w       jump to beginning of next word
b       jump to beginning of current or previous word
e       jump to end of current or next word
ge      jump to end of previous word

f{char} find next occurrence of char
F{char} find previous occurrence of char
;       find next occurrence
,       find previous occurrence

)(      move one sentence back/forward
}{      move one paragraph back/forward

gg      goto beginning of file, precede with number to jump to that line number
G       goto end of file

ctrl-f  page down
ctrl-b  page up
ctrl-d  half page down
ctrl-u  half page up
ctrl-y  line up
ctrl-e  line down
zt,zz,zb    position window so that cursor is on top of window, center of window or bottom


## Search and replace

:%s/foo/bar/g   search for foo in entire file (%s) and replace with bar
:%s/foo/bar/gc  search and replace with confirm (c)
replace %s in examples above with a range to limit replace to that range, e.g. 1,8 will execute on lines 1 to 8 while 1,$ is entire file, same as % above
/foo    search after cursor for foo
?foo    search before cursor for foo
:noh    remove highilghting from last search


## Cut and paste

c       change (cw: change word, c2w: change 2 words, c5h: change 5 characters back etc)
cc      change line
C       change to end of line
d       delete (works as c)
dd      delete line
D       delete to end of line
J       join following line with current
p       paste, prepend with " and char from :reg to paste that content
u       undo
y       yank, "+y yanks into register to paste outside of vim
yy      yank line

ctrl-w  delete word in insert mode
ctrl-u  delete line in insert mode

:reg    list registers with yanked content


## Assorted

.       repeat last command
ctrl-p  autocomplete from current file
:\d,\dw newfile.txt     saves lines between \d and \d in a new file called newfile.txt


## Buffers and files

:ls     list open buffers


## Settings

:set hlsearch               highlights search results
:set ignorecase             case insensitive search
:set incsearch              search incrementally when typing
:set relativenumber         sets numbers relative to current
:set smartcase              if search contains uppercase character, performs case sensitive search

