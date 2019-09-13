# Git

## 1. remote link to git

   `git remote show origin`

Output:

```
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
`git config --get remote.origin.url`

Output:

```
https://github.com/rajatissar/commands.git
```

## 2. Tags

```
Create an annotated tag`
```
`git tag -a <tag_name> -m '<tag_message>'`

```
Create a lightweight tag
```
`git tag <tag_name>`

```
Push all your tags (a regular push won't push a tag)
```
`git push origin --tags`

```
Push a single tag
```
`git push origin : <tag_name>`

```
List the tags in a repository
```
`git tag`

```
Remove a tag from a repository
```
`git tag -d <tag_name>`
`git push origin :refs/tags/<tag_name>`
