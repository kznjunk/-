- [Debian](#debian)
- [Vim](#vim)
- [Screen](#screen)
- [Domain](#domain)
- [Subdomain](#subdomain)
- [Knex](#knex)
- [NativeScript](#nativescript)
- [NGINX](#nginx)
- [Certbot](#certbot)
- [VueJS](#vuejs)
- [Github](#github)
- [Strapi](#strapi)
- [SQL](#sql)
- [Postgre SQL](#postgresql)
- [PM2](#pm2)
- [Wordpress](#wordpress)

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

- `ctrl + a, ESC, ` - scroll up/down

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
- Create linked files available/enabled: `ln -s /etc/nginx/sites-available/hello_there.conf /etc/nginx/sites-enabled/`

- Security by prompt
```
openssl passwd
sudo vim /etc/nginx/site_pass -> login:encodedPassword

location / {
  auth_basic "Admin Login";
  auth_basic_user_file /etc/nginx/site_pass;
}
```

- Security by IP
```
satisfy all;
allow 203.0.113.0;
allow 127.0.0.1;
deny all;
```

- `tail -f /var/log/nginx/error.log`

### Certbot
-----------


### VueJS
---------

- update vuejs: `npm install vue@latest`

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

- Merge into main
```
git checkout main
git merge dev
git push
```

- Existing repo to existing folder:
```
git init
git add .
git commit -m "First commit"
git remote add origin <remote repository URL>
git remote -v

git push origin main
OR
git branch -m <new_name>
git push origin -u <new_name>
OR 
git push origin -u -f <new_name>
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


### SQL
-------

- `mysql -u root -p`
- `SHOW DATABASES;`
- `SELECT host, user, password FROM mysql.user;`
- `FLUSH PRIVILEGES;`
- `$cfg['Servers'][$i]['AllowNoPassword'] = false;` && `$cfg['Servers'][$i]['AllowRoot'] = false;`
- `CREATE DATABASE example;`
```
CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON database_name.* TO 'user'@'localhost';
FLUSH PRIVILEGES;
```

- `sudo service mysql restart`


### PM2
-------

- `pm2 ls`
- `pm2 start bashscript.sh --name <app_name>`
```
$ pm2 restart app_name
$ pm2 reload app_name
$ pm2 stop app_name
$ pm2 delete app_name
```
- ``

Cluster Mode :

pm2 start app.js -i max


Ecosystem File :

pm2 ecosystem
to create a file

pm2 start ecosystem.config.js
pm2 start ecosystem.config.js --only "api-app,worker-app"
pm2 stop ecosystem.config.js
pm2 restart ecosystem.config.js
pm2 reload ecosystem.config.js
pm2 delete ecosystem.config.js

aas opposed to restart, which kills and restarts the process, reload achieves a 0-second-downtime reload.
```
pm2 reload <app_name>
pm2 reload process.json
pm2 reload process.json --only api
```


### Wordpress
-------

Clone website:
- Use the duplicator plugin
- Download both files
- FTP transfer them to the wanted folder
- Create a database with WP username/password
- Might need to `sudo chown -R www-data:www-data /var/www`
- Go to the url `/installer.php`



### NativeScript
-------

- CLI:
```
npm install -g nativescript

ns create my_app
ns preview

ns clean

ns run android
ns run ios

ns debug android
ns debug ios

ns doctor android
ns doctor ios
ns help

// Android Debug Bridge
adb devices
ns device

ns test init
ns test ios
ns test android
ns test android --watch
ns test ios --watch

// update
npm install -g nativescript
// at the root 
ns update

ns platform remove android
ns platform add android
ns platform remove ios
ns platform add ios

npm install @nativescript/core@latest --save
```

- Magic variables:
```
if (__DEV__)
if (global.isAndroid)
if (global.isIOS)
```


### Knex
-------

```
sudo knex migrate:rollback

knex migrate:make create_users_table
knex migrate:make create_planets_table

npm run migrate
npm run seed
npm run start
```

  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "migrate": "knex migrate:latest --knexfile src/db/knexfile.js",
    "seed": "knex seed:run --knexfile src/db/knexfile.js"
  },


### Postgresql
-------

```
sudo -u postgres psql
\l
\list or \l: list all databases
\c <db name>: connect to a certain database
\dt: list all tables in the current database using your search_path
\dt *.: list all tables in the current database regardless your search_path
```
