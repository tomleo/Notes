From Shell
==========

tmux attach -t <name of session>
    attach named session

tmux kill-session -t <name of session>
    kill session

tmux new -s <name of session>
    create new named session

tmux new -s <name of session> -d
    create new named session in background

tmux new -s <name of session> -n <name of first window>
    create new named session with named window


From TMUX Session
=================

Windows
-------

``PREFIX c``
    create new window

``PREFIX ,``
    rename window

``PREFIX <([0-9]+)>``
    switch to window x

``PREFIX f``
    find window by name

``PREFIX w``
    list windows

exit
    close window (closing all windows will completly close out the tmux session)

Panes
-----

``PREFIX %``
    split window vertically

``PREFIX "``
    split window horizontally

``PREFIX o``
    cycle through panes

``PREFIX <arrow key>``
    move around panes

``PREFIX :``
    enter command mode

Command Mode
------------

new-window -n <name>
    create new window with <name>  

new-window -n <name> "<command>"
    creates new window and executs command

    .. code-block:: cmd
        
        new-window -n process "top"

