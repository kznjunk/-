- [Debian](#debian)
- [Vim](#vim)
- [Screen](#screen)
- [Domain](#domain)
- [Subdomain](#subdomain)
- [NGINX](#nginx)
- [Certbot](#certbot)
- [VueJS](#vuejs)
- [Github](#github)
- [Strapi](#strapi)

### Debian
----------

- Updates:
```
sudo apt update -y
sudo apt upgrade -y
```

- Get size folders: `du -h --max-depth=1 /path/to/directory`

- Check disk spaces: `df -H`

- Change user password: `sudo passwd username`

- Change root password: `sudo su -` and `passwd`

- Enable/Disable root login over SSH
```
vim /etc/ssh/sshd_config

PermitRootLogin yes

service sshd restart
```

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

More at [aperiodic](http://aperiodic.net/screen/man:string_escapes) && [kilobitspersecond](https://www.kilobitspersecond.com/2014/02/10/understanding-gnu-screens-hardstatus-strings/)

### Domain
----------

Add/replace entry Type A to target your server IP

|Domain|TTL|Type|Target|
|---|---|---|---|
|example.com.|0|A|1.1.1.1|
|www.example.com.|0|A|1.1.1.1|

### Subdomain
-------------

Create subdomains by adding new Type A and CNAME entries
|Domain|TTL|Type|Target|
|---|---|---|---|
|dev.example.com.|0|A|1.1.1.1|
|www.dev.example.com.|0|CNAME|dev.example.com.|

### NGINX
---------

```
systemctl stop nginx
systemctl restart nginx
systemctl reload nginx
```

- Get version: `sudo nginx -v`
- Check the NGINX server is running: `curl -I 127.0.0.1`

### Certbot
-----------


### VueJS
---------
```
git clone git@github.com:project.git
npm i
npm run serve (dev)
npm run build (prod)
```

### Github
----------

- main
- dev
- myFeature
- release-*
- hotfix-*

- `git branch -a` - list all branches
- `git checkout -b dev` - create a new branch
- `git checkout -b myfeature dev` - create a new branch

- Creating a feature branch 
```
git checkout develop
git merge --no-ff myfeature
git branch -d myfeature
git push origin develop
```

- Creating a release branch
```
git checkout -b release-1.2 develop
./bump-version.sh 1.2
git commit -a -m "Bumped version number to 1.2"
```

### Strapi
----------

```
npm run build (to create the admin page)
npm run develop
```

- Dev to production steps:
  - commit the local project (without database) to a git repository
  - dump configuration from local project config:dump
  - dump my models collections in separated CSV files
  - clone the project on the production server
  - setup from yarn
  - restore configuration config:restore
  - import data from the CSV files into the new project
