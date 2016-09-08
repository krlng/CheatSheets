#Hackr Tools

##tmux

__Startig tmux__

```sh
tmux new -s name #create named tmux session
tmux attach -t 0 # attach the first session of tmux ls
```

__tmux commands__

```sh
d # detach
z # fullscreen view
% " #create panes
c #create windows
p n # switch to previous / next window
[ # enter copy mode (space: copy, enter: finish copy)
] # paste 

:source-file ~/.tmux.conf #reload tmux.conf
:swap-window -s 3 -t 1
:setw synchronize-panes on #run command in all panes
```

__commands based on my tmux.conf__

```sh
e # run command in all windows
a # toggle synchronize-panes
p # Copy tmux clipboard to default one
s # save settings
r # recover last settings
```

##vi



**Fast Lookup:**

```sh
yy          # copy line 
u           # undo
:%s/,/\r/g  # replace each , with a new line: 

x / dw / dd # Delete Character / word / line :
d$          # Delete all till end of line

5d          # remove line 5
5dd         # remove lines till line 5

:m +1 / -2  # move down / up one line

```

**Cursor Motion:**

```sh
hjkl    # Move cursor
L / M / H #Cursor to bottom / middle / top line
:\<NR>  # Go to file xy
w, e, b # start of next word, end of this word, backwards
2w      # jump to word forward
^       # Back to indentation    
0, $:   # start / end of the line
```

**Page Motion:**

```sh
gg               #Go to beginning of file
gG               #To to end of file
:                #Goto line

C-d              # Half page down
C-u              # Half page up
C-f              # Next page
C-b              # Previous page
C-Down or J      # Scroll down
C-Up or          # Scroll up
```
   
**Search:**
     
```sh
/         # Search forward          
n         # Search again            
?         # Search backward         
```

**Selection:**
     
```sh
Space     # Start selection         
Escape    # Clear selection         
Enter     # Copy selection          
p         # Paste buffer            
```

<kbd>:%y+</kbd> Copy full file to clipboard

![Vi Cheat Sheet](img/vicc.gif)


#less

-L #show line numbers

```
move window-wise: f / b
move half window-wise: d / u
Serach: / (n next search result)
Jump to first / last line: g / G
Find brakets by typing the brakets
```

## Shell Scripting

```sh
for f in ./bss_ddl/*; do hive --hivevar params_warehouse_hdfs=/bdhcore -f $f; done;
```

## xdotool

```sh
xdotool type mytext
```


##awk


```

```

##zsh


```
echo myPassword | sudo -S ls /tmp
cmd
```