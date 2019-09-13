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

- Create an annotated tag \\n
`git tag -a <tag_name> -m '<tag_message>'`

- Create a lightweight tag \\n
`git tag <tag_name>`

- Push all your tags (a regular push won't push a tag) \\n
`git push origin --tags`

- Push a single tag \\n
`git push origin : <tag_name>`

- List the tags in a repository \\n
`git tag`

- Remove a tag from a repository \\n
`git tag -d <tag_name>`
`git push origin :refs/tags/<tag_name>`
