## MY GIT WORKFLOW

- Update your local repository and create a **tmp** branch:
```console
dev> git pull
dev> git checkout -b tmp
```

- Make any changes you want in your local **tmp** branch:
```console
tmp> git commit -am "first"
tmp> git commit -am "second"
tmp> git commit -am "third"
```

- Reset your local repository to the original master branch, when **tmp** has been created:

```console
tmp> git reset --soft dev
```

or better (see why in [later section](#how-to-find-the-latest-common-commit-between-tmp-and-dev)):

```console
tmp> git reset --soft $(git merge-base tmp dev)
```


- Squash all your local changes in just one commit:

```console
tmp> git commit -m "feature-X-a.b.0 first"
```

- Switch to dev and pull any remote change, no merge should be required ad your **dev** branch didn't change yet:

```console
tmp> git checkout dev
dev> git pull
```

- In case of remote changes, a merge in **tmp** might be required, but it's better to rebase your local changes with the remote ones:

```console
dev> git checkout tmp
tmp> git rebase dev
```

- Handle any required merge in **tmp**, if needed, and compete the rebase process:

```console
tmp> git add --all
tmp> git rebase --continue
```

- Switch to **dev**, check whether new remote changes are available (redo the steps above to merge the new changes in **tmp** again) and merge the only commit from **tmp** (the **--ff-only** prevents merges when a fast-forward can't be applied):

```console
tmp> git checkout dev
dev> git pull
dev> git merge --ff-only tmp
```

- Remove the no longer needed **tmp** branch and push your commit to the remote repository:

```console
dev> git branch -d tmp
dev> git push
```

- If you are ready to release all the changes made so far, create a tag, merge with **dev** and push all:

```console
dev> git tag -a a.b.0 -m "version a.b.0"
dev> git push --tags
dev> git checkout release/a
release/a> git merge dev
release/a> git push --all
```

## HOW TO FIND THE LATEST COMMON COMMIT BETWEEN TMP AND DEV
If you are in trouble because you fetched remote changes in **dev** too early and you can't find the latest commit in common between **dev** and **tmp**, you can use the following command to find it out:

```console
tmp> git merge-base tmp dev
fcc0c1d6a3745be38783ef37f00ea9cdbf5168e8
```

Then you can just rest **tmp** to that commit and squash all your local changes in one: 

```console
tmp> git reset --soft fcc0c1d6a3745be38783ef37f00ea9cdbf5168e8
tmp> git commit -m "feature-X-a.b.0 first"
```


## HOW TO FIND OUT WHETHER DEV HAVE COMMITS THAT ARE NOT IN TMP
If you want to check whether **dev** has been updated with commits that are missing from **tmp**, you can run the following command:

```console
tmp> git log tmp..dev --oneline
d347b77 (HEAD -> dev, origin/dev) feature-C-a.b.0 first
```

## REFERENCES
- [GitFlow](https://nvie.com/posts/a-successful-git-branching-model/)
- [GitFlow considered harmful](https://www.endoflineblog.com/gitflow-considered-harmful)
- [One Flow](https://www.endoflineblog.com/oneflow-a-git-branching-model-and-workflow)
- [Trunk Based Development](https://medium.com/factualopinions/git-to-know-this-before-you-do-trunk-based-development-tbd-476bc8a7c22f)