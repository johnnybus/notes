# Git
##### Rename branch locally
    git branch -m old_branch new_branch
##### Delete the old branch
    git push origin :old_branch   
##### Push the new branch, set local branch to track the new remote
    git push --set-upstream origin new_branch

#### Git Flow
##### Initialise git flow on a repository
    git flow init
##### Create feature
    git flow feature start <feature name>
##### Finish feature
    git flow feature finish <feature name>
##### Publish a feature
    git flow feature publish <feature name>
##### Getting a published feature
    git flow feature pull origin <feature name>
##### Track a feature on origin
    git flow feature track <feature name>
##### Start a release
    git flow release start RELEASE [BASE]
##### Release branch after creating it
    git flow release publish RELEASE
##### Track remote release
    git flow release track RELEASE
##### Finish up a release
    git flow release finish RELEASE
##### Start an hotfix
    git flow hotfix start VERSION [BASENAME]
##### Finish an hotfix
    git flow hotfix finish VERSION


##### Useful URL’s:
- <https://zeroturnaround.com/wp-content/uploads/2016/02/Git-Cheat-Sheet.png>
- <http://danielkummer.github.io/git-flow-cheatsheet/>
