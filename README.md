# create new repo


```bash
$ git init
Initialized empty Git repository in /Users/sasank/projects/qure/random/git-submodule-try/test/.git/

$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
$ touch .gitignore
$ git add .gitignore
$ git commit -m "initial commit"
[master (root-commit) e8b0bde] initial commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 .gitignore
$ git log
commit e8b0bde60fae05ccc7c9dbc3eb5bc44f4a6bf60a (HEAD -> master)
Author: Sasank Chilamkurthy <sasankchilamkurthy@gmail.com>
Date:   Wed Oct 26 12:04:41 2022 +0530

    initial commit


$ git subtree  add --prefix knife  https://github.com/chsasank/Spoon-Knife bb4cc8d3b2e14b3af5df699876dd4ff3acd00b7f

# subtree merge master of https://github.com/octocat/Spoon-Knife into ours

$ git remote add knife https://github.com/chsasank/Spoon-Knife.git
$ git subtree merge --prefix knife knife/main

Merge made by the 'recursive' strategy.
 knife/README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

$ git fetch --all

Fetching knife
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 946 bytes | 946.00 KiB/s, done.
From https://github.com/chsasank/Spoon-Knife
 * [new branch]      super      -> knife/super


$ git subtree merge --prefix knife knife/super
```

