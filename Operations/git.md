# `git` Notes

## TL;DR

- Undo a commit

```bash
git push -f origin <commit-id>:<branch-name>
```

- Delete branch

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

- Managing remote entries

```bash
git remote -v
git remote add <name> <url>
git remote rm <name>
git remote rename <old_name> <new_name>
```

- Add private key to ssh agent

```bash
# start ssh agent
eval `ssh-agent -s`
# add key to gent
ssh-add ~/.ssh/id_rsa
```

## Contribute to an open source project

1. Fork the project repo and make a copy for yourself
2. Clone the repo to your local machine and boot up the development environment
3. Commit your code changes to git
4. Push your work to a feature branch in your fork
5. Open a PR against the original project repo

## Reference

- [3 Steps to Getting Started with Open Source](https://dev.to/ryan_c_harris/3-steps-to-getting-started-with-open-source-4f37)
- [Handy git guide from Atlassian](https://www.atlassian.com/git/tutorials/syncing)