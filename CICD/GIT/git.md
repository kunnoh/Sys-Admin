# Git
[Git](https://git-scm.com/) is a distributed version control software system. 

## Configuration
### Remotes
#### list all Git remotes
```sh
git remote -v
git remote show origin # Detailed config
```

#### Set remote URL
Add local repo to existing repo on [Github](https://github.com).  
```sh
git remote set-url origin git@github.com:<username>/<repo_name>.git
```

#### New branch from a commit
```sh
git log
```
Copy commit hash → e.g. **abc1234**.  
Create and switch to a new branch starting at that commit:  
```sh
git checkout -b new-feature abc1234
```



## Reference
1. [Git wikipedea](https://en.wikipedia.org/wiki/Git)  
2. [Git website](https://git-scm.com/)  
