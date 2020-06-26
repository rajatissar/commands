# Heroku

Heroku is a cloud platform as a service supporting several programming languages.

## Installation

```SH
$ sudo apt update
update apt
$ sudo apt install heroku
install heroku
```

OR

```SH
$ sudo snap install heroku --classic
install heroku by snap
```

OR

```SH
$ curl https://cli-assets.heroku.com/install.sh | sh
install heroku by curl
```

## Check Node Version

```SH
$ node --version
Check node version
```

## Check npm Version

```SH
$ npm --version
Check npm version
```

## Check Git Version

```SH
$ git --version
Check git version
```

## Show git remote repositories

```SH
$ git remote -v
Show git remote repositories
origin  https://github.com/rajatissar/commands.git (fetch)
origin  https://github.com/rajatissar/commands.git (push)
```

## Login Heroku account from heroku-cli

```SH
$ heroku login
This will used to login Heroku account from heroku-cli
```

## Add Heroku repository to your Git project

```SH
$ heroku git:remote -a repository_name
This will add heroku repository to your current project
```

## Deploy your changes to Heroku

```SH
$ git push heroku master
This will deploy your changes to master branch on heroku
$ git push heroku branch_name:master
This will deploy your changes from branch_name branch(local) to master branch(heroku)
```

## Creating Heroku App

```SH
$ heroku create app_name
It will create a new app by name app_name
```

## Check Heroku Logs

```SH
$ heroku logs
Shows Heroku logs
```

## Clone Heroku repository (Specific branch)

```SH
$ heroku git:clone -a heroku_branch_name
Clone Heroku repository
```

## Add Heroku Git to your repository

```SH
$ git remote add heroku https://git.heroku.com/heroku_branch_name.git
Add Heroku Git to your repository
```
