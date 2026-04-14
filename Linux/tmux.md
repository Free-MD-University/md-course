# Sessions
Start new: tmux
Start named: tmux new -s <name>
Detach: C-b + d
Attach: tmux a
Attach named: tmux a -t <name>
List sessions: tmux ls
Kill session: tmux kill-session -t <name> 

# Windows (Tabs)

New window: C-b + c
Next window: C-b + n
Previous window: C-b + p
Rename window: C-b + ,
List windows: C-b + w 

# Panes (Splits) 
Split vertical: C-b + %
Split horizontal: C-b + "
Switch pane: C-b + <arrow key>
Toggle layout: C-b + <space>
Close pane: C-b + x 