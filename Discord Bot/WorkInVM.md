# Setting Up the Bot on the VM

1. Install discord.py and other requirements on the VM
```
pip install -U discord requests
```

2. Create a python file using your preferred command line editor and copy your code into there.

If you are using vim, you open/create a file using `vim <filename>`. To go into edit mode, click `i`. To save your file, click `ESC` followed by typing `:wq` and then pressing enter.

3. Install tmux using `sudo apt install tmux`.
4. Create a tmux session by typing in `tmux new -s mysession`.
5. Run your bot file using `python3 filename`
6. If you close out of your terminal, you'll notice that the bot is still running.
7. If you want to go into your tmux session again, type in `tmux attach -t mysession` and if you want to kill a session, type in `tmux kill-session -t mysession`.

A list of all tmux commands is avaible on https://tmuxcheatsheet.com.
