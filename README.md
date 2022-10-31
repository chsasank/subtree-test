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
Auto-merging knife/index.html
CONFLICT (content): Merge conflict in knife/index.html
Automatic merge failed; fix conflicts and then commit the result.

# manually fix conflicts
$ git add knife/index.html

# merge commit
$ git commit
[master b437da0] Merge commit 'd5115e11c928081e2288e926c495b74892160202'
```


## PR flow


```
$ git checkout -b pull/batman
```


## Bibucket LFS import

```
$ git checkout -b bb/lfs
$ git remote add bb_lfs git@bitbucket.org:chsasank/knife_bb.git
$ git status
On branch bb/lfs
nothing to commit, working tree clean

$ git fetch --all
$ git subtree add --prefix bb_lfs_subtree bb_lfs master
git fetch bb_lfs master
From bitbucket.org:chsasank/knife_bb
 * branch            master     -> FETCH_HEAD
Downloading bb_lfs_subtree/Transaction Cost Economics and .pdf (784 KB)
Error downloading object: bb_lfs_subtree/Transaction Cost Economics and .pdf (109b189): Smudge error: Error downloading bb_lfs_subtree/Transaction Cost Economics and .pdf (109b189cfb462e69bf55c91acb32f1d73dff1c42c6df29e9686abf99fb7101d6): [109b189cfb462e69bf55c91acb32f1d73dff1c42c6df29e9686abf99fb7101d6] Object does not exist on the server: [404] Object does not exist on the server

Errors logged to /Users/sasank/projects/qure/random/git-submodule-try/test/.git/lfs/logs/20221026T144926.177464.log
Use `git lfs logs last` to view the log.
error: external filter 'git-lfs filter-process' failed
fatal: bb_lfs_subtree/Transaction Cost Economics and .pdf: smudge filter lfs failed

```

Work around: https://arwedus.wordpress.com/2021/09/09/avoid-missing-git-lfs-files-on-subtree-merge/

```
$ git clone git@bitbucket.org:chsasank/knife_bb.git
$ cd knife_bb
$ git remote add github git@github.com:chsasank/subtree-test.git
$ git lfs push github master --all
```

And it works now!

```
$ cd subtree_test
$ git subtree add --prefix bb_lfs_subtree bb_lfs master
git fetch bb_lfs master
From bitbucket.org:chsasank/knife_bb
 * branch            master     -> FETCH_HEAD
Added dir 'bb_lfs_subtree'
```



## Subtree: squashing commits and merging

Create a repo to squash subtree merge
```
$ git clone git@github.com:chsasank/subtree-squash-merge.git
$ cd subtree-squash-merge
$ git log --graph --oneline --branches 
* aa041b5 (origin/branch/B2, branch/B2) commit D
* 5008afc commit E
| * 14076b6 (origin/branch/B1, branch/B1) commit D
| * 3515a4b (HEAD -> main, origin/main) commit C
|/  
* 9030f6a commit B
* 9d157f3 commit A
```

Now let's add `main` to our repo using sqash

```bash
$ cd subtree-test
$ git checkout -b squash_merge
$ git remote add subtree-squash-merge git@github.com:chsasank/subtree-squash-merge.git
$ git subtree add --prefix subtree-squash-merge --squash subtree-squash-merge main
$ git push origin squash_merge
```

Now let's see how the PR looks like: https://github.com/chsasank/subtree-test/pull/5. Let's merge this for now.

Now time to do some black magic! We want to merge `branch/B1` and `branch/B2` into our repo! Here's our graph:

```
A - B - C (main) - D (branch/B1)
    \- E - F (branch/B2)
```

### Merge B1

We have squashed all the commits until C (master). Let's start with obvious merge: `branch/B1`

```bash
$ cd subtree-test
$ git checkout master
$ git checkout -b squash_merge_B1

# note the lack of squash in the merge.
$ git subtree merge --prefix subtree-squash-merge subtree-squash-merge/branch/B1
fatal: refusing to merge unrelated histories

# now with squash in the merge.
$ git subtree merge --prefix subtree-squash-merge --squash subtree-squash-merge/branch/B1
$ git log --graph --oneline 
*   57f22e7 (HEAD -> squash_merge_B1) Merge commit 'd37818beac523cc42754f3548bf19a32b7f2640' into squash_merge_B1
|\  
| * d037819 Squashed 'subtree-squash-merge/' changes from 3515a4b..14076b6
* |   20a3e65 (master) Merge pull request #5 from chsasank/squash_merge
|\ \  
| * | 54a1af3 Merge commit '33edce7847bab2a0932df8d53e46378b248e6039' as 'subtree-squash-merge'
|/| | 
| |/  
| * 33edce7 Squashed 'subtree-squash-merge/' content from commit 3515a4b
*   7b76356 Merge pull request #3 from chsasank/squash_merge

$ git push origin squash_merge_B1
```

Let's create PR and see how the diff looks like: https://github.com/chsasank/subtree-test/pull/6

### Merge B2

```bash
$ cd subtree-test
$ git checkout master
$ git pull
$ git checkout -b squash_merge_B2

# note the lack of squash in the merge.
$ git subtree merge --prefix subtree-squash-merge subtree-squash-merge/branch/B2
fatal: refusing to merge unrelated histories

# now with squash in the merge.
$ git subtree merge --prefix subtree-squash-merge --squash subtree-squash-merge/branch/B2

$ git push origin squash_merge_B2
```

Now if we raise PR: https://github.com/chsasank/subtree-test/pull/7