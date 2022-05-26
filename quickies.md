### Debian
----------

- Get size folders:
`du -h --max-depth=1 /path/to/directory`

- Check disk spaces:
`df -H`

- Change user password
`sudo passwd username`

- Change root password `sudo su -` and `passwd`

### Vim
-------

- `ctrl + f` - page down
- `ctrl + b` - page up
- `ctrl + d` - scroll window downwards, normally half a a screen
- `ctrl + u` - scroll window upwards, normally half a a screen

### Screen
----------

- `screen -S mySession`
- `screen -ls`
- `screen -r mySession`
- `ctrl + a c` - new tab
- `ctrl + a A` - give it a new name
- `ctrl + a "` - list all tabs
- `ctrl + a a` - remove tab
- `ctrl + a ctrl + d` - detach screen
- `screen -X -S [session # you want to kill] kill or quit`


- Customize the status bar in `/etc/screenrc` or `~/.screenrc`
- `ctrl-a : source ~/.screenrc` - reset screen with a new config file 
- `hardstatus string '%{gk} %{wk}%?%-Lw%?%{=b kR}(%{W}%n*%f %t%?(%u)%?%{=b kR})%{= w}%?%+Lw%?%? %{g}- %{d}%l%{g} - %{= w}%D %d/%m/%Y %0c:%s%{g} %{W}'`

http://aperiodic.net/screen/man:string_escapes

https://www.kilobitspersecond.com/2014/02/10/understanding-gnu-screens-hardstatus-strings/
