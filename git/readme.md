# Git

## 1. Git Remote link

```bash
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

```bash
$ git config --get remote.origin.url
https://github.com/rajatissar/commands.git
```

## 2. Tags

```bash
$ git tag -a <tag_name> -m '<tag_message>'
Create an annotated tag
```

```bash
$ git tag <tag_name>
Create a lightweight tag
```

```bash
$ git push origin --tags
Push all your tags (a regular push won't push a tag)
```

```bash
$ git push origin : <tag_name>
Push a single tag
```

```bash
git tag
List the tags in a repository
```

```bash
$ git tag -d <tag_name>
$ git push origin :refs/tags/<tag_name>
Remove a tag from a repository
```

## 3. config

```bash
$ git config user.email
Sets email of user
$ git config user.name
Sets name of user
```
