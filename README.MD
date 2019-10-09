## WORKFLOW

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

- Squash all your local changes in just one commit:

```console
tmp> git commit -m "feature-X-a.b.0 first"
```

- Switch to dev and pull any remote change, no merge should be required:

```console
tmp> git checkout dev
dev> git pull
```

- In case of remote changes, a merge in **tmp** might be required, but it's better to rebase your local changes with the remote ones:

```console
dev> git checkout tmp
tmp> git rebase dev
```

- Handle any required merge in **tmp**, if needed:

```console
tmp> git add .
tmp> git rebase --continue
```

- Switch to **dev**, check if new remote changes are available (redo the steps above to merge the new changes in **tmp** again) and merge the only commit from **tmp**:

```console
tmp> git checkout dev
dev> git pull
dev> git merge tmp
```











