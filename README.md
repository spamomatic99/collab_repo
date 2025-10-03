
# How to collaborate on Github



## Authenticating on Github

Before you can interact with github by pushing changes to your repos, you need to establish a way to authenticate with github. For some time now, it is not anymore possible to use username and password. Instead, you should upload an ssh public key in your account. If you don't know what I am talking about, please [read this page](./ssh_keys.md), otherwise, just read on. 



## Github basic actions

Everything starts by creating a new repo on the github account. For instance [the one we are currently working with](https://github.com/leonardocerliani/collab_repo/tree/main). Upon creation, github will give you a list of command to run in the terminal so that you can create your own local version of the online repo, connect it to the remote version (on github) and start to write, commit and push online

```bash 
# For instance if you called the repo myNewRepo on the github website,
# mkdir myNewRepo and change dir into it (not necessary but highly recommended)
mkdir myNewRepo
cd myNewRepo

# This is what github prompts you to do next
#	Write a first header into a README.md file (will be displayed by default on the website)
echo "# myNewRepo" >> README.md

# initialize a repo locally
git init  

# add the README.md file you just created
git add README.md

# commit the change
git commit -m "first commit"

# switch to the main branch
git branch -M main

# add the remote location to sync the local repo with
git remote add origin git@github.com:leonardocerliani/myNewRepo.git

# push it to the remote repo, so that it will be displayed in github
git push -u origin main
```

It can seem like a handful, and indeed it is, but for now just execute those lines. It needs to be done only once.

In there, there are three git commands which represents what you will use  90% of the time:

- `git add .` : add all the files in the current dir(ectory) and subdir. These are all the files you want to track and stage for commit. If you have made a mistake (for instance you added some files that you do not want to track or appear on the github repo - see later `.gitignore`), you can do `git reset`.
-  `git commit -m "added a comment to the README.md file"` : now you create the actual snapshot that will be pushed to the website. Again in case of mistakes, you can do `git reset` but _only before having pushed to the remote repo_.
- `git push` : with this you upload new files or the changes you made to existing files to the github website.



## Adding a `.gitignore`

Often overlooked by beginners, this is probably the best feature of github IMHO. Let's say that you have your local repo in a folder in which there are also LLM API keys or data which are either very large or you just don't want to share. In this case you can add them to a `.gitignore` file - that you need to create - and they will stay only in your local repo. You can add both files and directories (name ending in `/`), and also use wildcards

```bash
# example of a .gitignore file

# ignore my_api_key.txt
my_api_key.txt

# ignore all the folders whose name starts with data
data*/

```



In the specific case that you want to check if you added some large files by mistake, you can run this

```bash
git diff --cached --name-only | xargs -I {} du -h {} | sort -hr
```

Remember that github rejects to push all files > 100MB





## Collaborators vs Contributors paradigms

A basic concept is that a repo has a `main` branch, where the main / production / live code is, and then several __branches__ which are not part of the main code. These are copies of the whole repo at a certain point in time, and are used by single users to work e.g. on new features of the code independently, even if the main code undergoes changes in the meanwhile.

There are two main ways in which a group can collaborate on a shared repo. 

- __Collaborators__: The owner of the repo - likely the person who created it - invites other users to collaborate. In this case everybody works on the same repo, likely in different branches.

- __Contributors__: The owner of the repo did not invite any collaborator, however anybody can __fork__ the repo - i.e. create a copy of the repo at one point in time - in her own github account and work on in independently. 

In both cases, when a user is done working on a new feature, she can ask to merge it with the main code by submitting a __pull request__. If this is accepted, it will be __pushed__ to the main code. However there is a big difference between the two approaches.

__Collaborators__ must make sure that they are working on their own branch, so that when they push modifications to github, they will not touch the main code, for which an authorized pull request is necessary. In case they make a mistake - typically forgetting to checkout on their own branch - they might pull it directly to the main code and potentially create conflicts or disasters.

Instead, when you are a __Contributor__, it means that you are working on your own personal copy of the repo, and you are not authorized to directly push to the main code of the main repository. You can only push to the main code of _your own copy_ of the repo. In this way, you are sure that if you make a mistake (e.g. deleting a folder in the main), there will always be a main code you can synchronize your repo with.

To summarize:

**Collaborators**

- have write access to the main repo
- can push, merge, manage issues and pull requests
- are added by the owner through invitation

**Contributors**

- do NOT have write access to the main repo, only to their own fork (copy) of the main repo
- contribute via pull requests
- changes must be reviewed and accepted (merged) by repo owner (and/or her collaborators if any)

Personally I find the latter modality more safe for beginners, exactly because of the aforementioned reasons. In addition, should be owner of the main repo make a mistake herself, there is still a good chance that one of the contributors has a safe copy of the whole repo that can be reinstated in the main repo.

Of course there are ways to "go back in time" and to prevent collaborators to directly push to main, but again for a beginner situation, I still think that the contributors paradigm is the safests and - also from the perspective of the main repo owner / maintainer - the easiest to manage.

**For this reason, I will propose below how the interaction on a repo can be carried out using the contributors paradigm, based on repo forks.**







