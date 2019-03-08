# `git` Notes

* Undo a commit

`git push -f origin <commit-id>:<branch-name>`

* Delete branch

**For deleting the remote branch:**

```base
git push origin --delete <your_branch>
```

**For deleting the local branch, you have three ways:**

```bash
# 1
git branch -D <branch_name>

# 2
git branch --delete --force <branch_name>  //same as -D

# 3
git branch --delete  <branch_name>         //error on unmerge
```

** ssh agent

```bash
# start ssh agent
eval `ssh-agent -s`
# add key to gent
ssh-add ~/.ssh/id_rsa
```