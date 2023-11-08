[![en](https://img.shields.io/badge/lang-en-red.svg):ballot_box_with_check:](#) [![es](https://img.shields.io/badge/lang-es-yellow.svg):black_large_square:](https://github.com/SpaikSaucus/cheatsheets/blob/main/DevOps/ControlVersion/README.es.md)

# CheatSheets Control Version

## Table of Contents
- [Web Semantic Version](#web-semantic-version)
- [Git Branching Strategies](#git-branching-strategies)
- [Tortoise GIT configuration Windows](#tortoise-git-configuration-windows)
- [TFS Remove workspace](#tfs-remove-workspace)

## Web Semantic Version
* Click here [semver.org](https://semver.org/)


## Git Branching Strategies

### GitHub Flow
__Is ideal for organizations that need simplicity, and roll out frequently.__ 
Every unit of work, whether it be a bugfix or feature, is done through a branch that is created from master. After the work has been completed in the branch, it is reviewed and tested before being merged into master and pushed out to production.

<img style="background-color:white;" src='https://github.com/SpaikSaucus/cheatsheets/blob/main/DevOps/ControlVersion/git-branching-strategies--github-flow.png?raw=true' />

### Git Flow
__Is ideally suited for projects that have a scheduled release cycle.__
Consistes of two "main" branches that last forever. These are master and develop. 
* The master branch is the main branch where the source code of HEAD always reflects a production-ready state (e.g. your releases)
* The develop branch always has the latest changes that are ready for testing in a release. This is your "nightly", "unstable" line.

<img style="background-color:white;" src='https://github.com/SpaikSaucus/cheatsheets/blob/main/DevOps/ControlVersion/git-branching-strategies--git-flow.png?raw=true' />

### GitLab Flow
__This is the ideal workflow for organizations that need to release frequently, rather than having scheduled releases.__

Unlike the other flows where master was the branch that represents the code that is on the production server, this flow has a dedicated __production__ branch that serves that purpose. Additionally, a __staging__ branch is included to represent your staging environment that you enter for more extensive testing and even end-user testing. This means that you must have at least 3 main branches:

* master: Branch of everyone's local development environment.
* staging: this is where the master is merged for testing before going to production.
* production: This is the production code tagged with the release.

<img style="background-color:white;" src='https://github.com/SpaikSaucus/cheatsheets/blob/main/DevOps/ControlVersion/git-branching-strategies--gitlab-flow.png?raw=true' />


#### References:
* https://blog.programster.org/git-workflows
* https://www.linkedin.com/posts/nikkisiapno_git-branching-strategies-explained-a-well-planned-activity-7119985969348448256--Kgo


## Tortoise GIT configuration Windows
1. Run the __CMD.exe__ console and locate it in the C:\Users\\[myUser] folder
2. Run the command:
     * ssh-keygen -t ed25519 -C "my_mail@gmail.com"
     (the email must be the one used in github)
3. Run the App __puttyGen__ :arrow_right: conversions :arrow_right: import key :arrow_right:
C:\Users\\[myUser]\\.ssh\id_ed25519 :arrow_right: Save privateKey :arrow_right: name id_ed25519_ppk
4. At https://github.com/settings/keys
     click __New SSH Key__ and copy the contents of the file:
     * C:\Users\\[myUser]\\.ssh\id_ed25519.pub
5. Go to the repository and copy the line to clone the repo, but select SSH.
Example: git@github.com:SpaikSaucus/cheatsheets.git
6. Go to our workspace and right click, git clone and check __LoadPuttyKey__ to load the file generated in step 3:
     * C:\Users\[myUser]\.ssh\id_ed25519_ppk.ppk
7. In the push, click on __Autoload Putty Key__.


## TFS Remove workspace 
__Required admin permissions at TFS__
* C:\Program Files\Microsoft Visual Studio 14.0>tf workspace /delete "C0125826E;UserPerez"