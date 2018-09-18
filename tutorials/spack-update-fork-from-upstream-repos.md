# Spack: A possible workflow for updating packages

#### Contributed by [Steve Hudson](https://github.com/shuds13)

#### Publication date:  Sept 17, 2018

### Introduction

This article assumes you have already made a github fork from spack and cloned it to your local system.

[How to create forks](https://help.github.com/articles/fork-a-repo)

You now have a configuration like [this](<https://www.google.com/search?q=github+fork+repo&safe=off&client=ubuntu&hs=or1&channel=fs&source=lnms&tbm=isch&sa=X&ved=0ahUKEwjn4u-l38LdAhUp5oMKHeEaBHQQ_AUIDigB&biw=1279&bih=753#imgrc=5wGOHz29WSUMhM>]

Upstream is the proper Spack repo on github. Origin is your fork on github and local machine
is your clone (from your fork).

Note: Details relating to Spack may change at any time, including whether develop is
used as the default branch.


### DO ONCE in your local checkout:

To set upstream repo:

    git remote add upstream https://github.com/spack/spack.git
    git remote -v # check added

(Optional) Prevent accidental pushes to upstream:
    git remote set-url --push upstream no_push
    git remote -v # Check for line: `upstream	no_push (push)`
    

### Now to update (the main develop branch):

You will now update your local machine from the upstream repo (if in doubt - make a copy of local repo
in your filestystem before doing the following).

Check upstream remote is present:
    git remote -v

Ensure you are on the develop branch:
    git checkout develop

Fetch from upstream repo:
    git fetch upstream

Now to update your local machine you may wish to rebase or overwrite your local files.
Select from the following:

If you have local changes to go "on top" of latest code.
    git rebase upstream/develop

Or to make your local machine identical to upstream repo (**WARNING** Any local changes WILL BE LOST):
    git reset --hard upstream/develop

    
(Optional) You may want to update your forked (origin) repo on github at this point.
This may requires a forced push:
    git push origin develop --force
    

### Making changes

You can optionally create a branch to make changes on. This may be a good idea, especially if
you have multiple packages, to make separate branches for each package.

See the Spack [packaging](https://spack.readthedocs.io/en/latest/packaging_guide.html) and
[contibution](https://spack.readthedocs.io/en/latest/contribution_guide.html) guides for more info.


Quick example to update MYPACKAGE:
    git branch update_MYPACKAGE
    git checkout update_MYPACKAGE
    spack edit MYPACKAGE
    spack checksum MYPACKAGE

Update MYPACKAGE file by pasting in the new checksum line.
If `spack checksum` does not work, get the tarball for the new release and use `sha256sum MYPACKAGE.tar.gz`

Check package:
     spack flake8

If OK add, commit and push to origin (forked repo)
     git commit -am "Update MYPACKAGE"
     git push origin update_MYPACKAGE --force
     
*Note spack can be found in the repo at bin/spack

Once the branch is pushed to the forked repo - go to github and do a pull requrest from this
branch on the fork to the develop branch on the upstream.

    
### Express summary: Make fork identical to upstream

Quick summary for bringing develop branch on forked repo up to speed with upstream
(YOU WILL LOSE ANY CHANGES):
    git remote add upstream https://github.com/spack/spack.git
    git fetch upstream
    git checkout develop
    git reset --hard upstream/develop
    git push origin master --force


Reference: <https://stackoverflow.com/questions/9646167/clean-up-a-fork-and-restart-it-from-the-upstream/39628366>

<!---
Publish: Yes
Categories: development
Topics: configuration and builds, deployment
Tags: 
Level: 2
Prerequisites: default
Aggregate: stand-alone and subresource
--->
