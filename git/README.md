# Branch

- dev 
- pre prod 
- prod

### Create branch
```
git branch <branch_name>
```
### Get current branch
```
git branch
```
### Checkout branch
```
git checkout <branch_name>
or 
git switch <branch_name>
```

#### Remote repository
- master -> don't have access on it only admin
- branch1
#### Local repository
- master  -> so if work on master, you can't push to master
- branch1 -< create branch1 and work on it


#### Merge 
Using pull request

```

# open remote repository
# push to your branch 
# create pull request so admin can merge it


```

merge on local repository

```
git switch
<branch_name_you_want_to_merge_on_it>

git merge <branch_name_you_want_to_merge>
```


#### google it
> What is git cherry-pick 
>> Git cherry-pick 


# Tagging
Use tag to store version

- lightweight
- annotated
  
### List tag
```
git tag --list
```
### Create tag
```
git tag v1.0.1
```
### Push tag to remote repository
```
git push origin <tag_name>
```

### Delete tag
```
git tag -d <tag_name>
```
### annotated tag
```
git tag -a <tag_name> -m "message"

git tag -a v1.0.1 -m "annotated tag"
```

```
git show <tag_name>
```
