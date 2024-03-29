# Git

Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

## 1. Clone

```BASH
$ git clone https://github.com/rajatissar/git.git
It will take clone
```

```BASH
$ git clone --branch=feature https://github.com/rajatissar/git.git customFolderName
It will take clone of specific branch and save in specific folder
```

## 2. Set config of Git locally

```BASH
$ git config user.email
Sets email of user
$ git config user.name
Sets name of user
```

## 3. Get information about remote repository

```BASH
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/rajatissar/commands.git
  Push  URL: https://github.com/rajatissar/commands.git
  HEAD branch: master
  Remote branches:
    develop        tracked
    master         tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

```BASH
$ git config --get remote.origin.url
https://github.com/rajatissar/commands.git
```

## 4. Steps to be followed for converting all previous commits into single commit

```BASH
$ rm -rf .git
It will delete the local .git folder present in your workspace

$ git init
It will initialize a new git repository locally

$ git add .
It will add the current directory to the staging area of your git repository

$ git commit -m "initial commit"
It will add a commit message

$ git remote add origin https://github.com/rajatissar/node-server.git
It will add a remote git repository of your choice

$ git push --force --set-upstream origin master
If the remote repository is same then you need to push the commit by force
```

## 5. Delete branch

```BASH
$ git branch -d development
It will delete the development branch present locally on system. It will not delete branch on remote
```

```BASH
$ git push origin --delete development
It will delete the development branch present on remote repository
```

## 6. Tags

```BASH
$ git tag -a <tag_name> -m '<tag_message>'
Create an annotated tag
```

```BASH
$ git tag <tag_name>
Create a lightweight tag
```

```BASH
$ git push origin --tags
Push all your tags (a regular push won't push a tag)
```

```BASH
$ git push origin : <tag_name>
Push a single tag
```

```BASH
git tag
List the tags in a repository
```

```BASH
$ git tag -d <tag_name>
$ git push origin :refs/tags/<tag_name>
Remove a tag from a repository
```

## 7. Move stage code to unstage code

```BASH
$ git reset
It will move code from stage to unstage
```

## 8. Discard not commit code

```BASH
$ git checkout .
It will discard changes in changed files
```

```BASH
$ git clean -fd
It will remove untracked files
```

### 9. Remove latest pull changes

```BASH
$ git reset --soft HEAD~1
It will remove latest pull
```

### 10. Remove latest commit

```BASH
$ git reset --soft HEAD~1
It will remove latest commit
```

### 11. Remove latest commit on remote as well

```BASH
$ git reset HEAD~
It will remove latest commit

$ git push origin master --force
It will remove latest commit on remote
```
