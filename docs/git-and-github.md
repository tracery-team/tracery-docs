# Git and Github

Github is the central part of our
collaboration. Our application source code
is hosted under the single organization called
[Tracery Team](https://github.com/tracery-team).

We have 3 main repositories:

1. [Documentation](https://github.com/tracery-team/tracery-docs)
2. [Front-End](https://github.com/tracery-team/tracery-frontend)
3. [Back-End](https://github.com/tracery-team/tracery-backend)

## Getting Started

### Git
First of all, you need to install Git itself.
You can run the following command to check whether
you have it installed on your system:
```sh
git --version
```
if git is not found, then follow [these](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) instructions

Git is the only obligatory program. And if you are
using modern IDEs, only Git should be enough. Many of
modern IDEs have a good Git integration. So, I encourage you
to use the WebStorm JetBrains product. It will help you
to track your Git history, pull changes from the remote,
and resolve conflicts.

JetBrains offers free licenses for students,
so you should figure out how to get one.
I got mine from [GitHub Education pack](https://education.github.com/pack).
You can get WebStorm here: [Download Link](https://www.jetbrains.com/webstorm/)

[VSCode](https://code.visualstudio.com/) is another option. You will have to 
spend more time installing plugins, so when you install the IDE itself, search
for Git plugins available on the Extension marketplace.

However, there are still some useful programs
that you may consider installing:

### GitHub Desktop
I personally don't use it, but this tool
may simplify the git and github workflow for you.
Here you can download it: [Download Link](https://github.com/apps/desktop)

### Sourcetree
Again, I personally don't use it, but
Sourcetree shows you a beautiful graph
of branches and commits. Sometimes it helps
in understanding what's going on in the
repository. Pretty nice program. 
You can install it here: [Download Link](https://www.sourcetreeapp.com/)

### GitGraph
I indeed use it. Console application 
that shows you beautiful Git branch&commit
graphs. You can install it here: [Download Link](https://github.com/mlange-42/git-graph)

## How to use Git?
My personal advice: use git + gitgraph from command line,
and your IDE for conflicts resolution. All other tools
introduce unnecessary complexity. Command-line Git
itself is simple enough, and you don't need any other
programs to be successful.

### List of most popular commands

Here is the list of commands that you will be using:
```sh
# To download a repository locally
git clone https://github.com/tracery-team/{repository}

# Switch to git branch of your need
git checkout <branch>

# Check what has changed on GitHub - maybe someone added a new branch
git fetch

# Apply changes on GitHub to your current branch
git pull

# Check the status of your changes
git status

# See the changes (it will show you the changes in code)
git diff

# Add files to staging area - before commit
git add . # all files
git add src/* # only needed files

# Commit changes - adds a comit to your history
git commit -m "feat(Header): added styling"

# Before commit, you may want to delete some file
# from staging area, if you don't want to share it
git rm <file>

# Post your changes on GitHub
git push

# Discard changes, rollback to the last commit
git stash

# If you messed up something, rollback to any commit
git reset --hard <commit>

# If you want to apply changes from another branch, do
git merge <branch>
```

### Popular scenarios

#### Adding a feature
```sh
# You created a new issue on github with a branch
# and you need to work on something using that branch

# Get the remote changes from github
git fetch
git checkout feature-branch

# Now you are on the feature-branch

# Coding...
# Coding...

# Check what you did:
git status
git diff

# Add your changes to staging area:
git add src/my-feature/*

# Check whether everything is OK:
git status

# Committing changes:
git commit -m "feat(src/my-feature): added a feature"

# Let everybody know about your changes,
# publish them online on GitHub:
git push
```

#### Downloading Changes
```sh
# Imagine somebody did the previous scenario,
# and then merged their changes to `main` branch.
# You have an old `main` branch locally,
# and you would like to update it

# First of all, check whether
# you have any remote changes
git fetch

# Normally it will show you
# what has changed remotely

# Now, you need to switch to your
# main branch
git checkout main

# Update the main branch
git pull

# Now you have an up-to-date main branch
```

#### Using changes from other branches
```sh
# Imagine that someone is working on a branch
# called feature-2. They implemented things that you
# wish you had on your branch

# merge their branch into your current branch
git merge feature-2

# Oh no! There are some conflicts.
# Resolve the conflicts using your IDE

# Continue the merge
git merge --continue
```

## To .gitignore or not to .gitignore
Some files **_should not be tracked_** by git.
They should nor be present in your local history,
neither on GitHub.

This includes:

1. Environment variables files like .env, .env.runtime, etc.
2. Files created by your IDE, like .idea, .vscode, etc.
3. Files created by your programming platform, like node_modules
4. Files created by your operating system, like .DS_Store
5. Cache files

And many, many more. The reason we don't want them
to be on our version control system could be one of the following:

1. They are large (node_modules can contain hundreds of megabytes)
2. They contain sensitive data (like .env files, where you put passwords and API keys)
3. They are unnecessary (everybody uses a different IDE, so why should we have your IDE files in our repo?)
4. They may lead to inconsistencies

There are two ways to deal with those files:

1. Add them to .gitignore (**_the preferred way_**)
2. Not adding them to staging area before commit: they will be excluded from commmit

## Conventional Commits
When you do a commit, you can **_and must_** attach a message using *-m* flag.
However, without proper rules on **_how to write_** your commit messages,
they will quickly become pretty random.

And here come the Conventional Commits
You can read about them [here](https://www.conventionalcommits.org/en/v1.0.0/).
To put it shortly, it's a convention that tells you how to
properly write commit messages

Bad commit messages:

1. small changes
2. fixed the bug
3. added styling
4. commit commit

Good commit messages:

1. feat(footer): changed application footer
2. fix(navigation): removed dead links from the navigation
3. docs(src/components/CronPicker): added documentation for arguments

## Clear and Intuitive Collaboration
There are best practices on how to work
on a project together.

Imagine if everybody was using the same branch.
You make changes, you commit, you push... and boom!
You get a notification that your branches are **_divergent_**.
What could happen:

1. Last commit on `main` branch is `A`
2. You and Mateusz simultaneously start working on that branch
3. Mateusz finishes first, he commits his changes as commit `B`
and immediately pushes it to GitHub
4. Remotely, `main` branch has a history of `A -> B`
5. You finish your changes and commit them as commit `C`
6. Your local history of `main` looks like `A -> C`
7. When you push it to GitHub, it gets confused on what happened:
how to treat the fact that you dont have the `B` commit?

Even when working in a pair, bad git practices can lead to
serious consequences.
So, people invented something which is called 
[Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow):
a tool and a set of rules that tells you how to organize your git
collaboration on a project.

We won't use Gitflow, but we will have some rules
on how we work with our repositories,
so we won't have to resolve divergent branches and merge conflicts.

### Our plan of work

1. We need a feature. I, as a team leader, create an Issue on Github,
assign some priority to this issue, and assign a person responsible for
this issue.
2. You, the person assigned to the issue, go to GitHub,
click 'Create branch for the issue', git fetch & git checkout to
that branch locally, and start working on that feature on your
separate branch
3. **_This is YOUR branch, and noone should modify it except
of you!_** Only one person is working per branch.
4. After you've done with your feature, you merge the `main` branch
to your branch, and resolve conflicts
5. After the conflicts are resolved, you create a **_Pull Request_** on GitHub
6. I check the PR, and make a code review. You change the code if
it doesn't meet the criteria
7. When I assume that the code is good enough, I merge your PR to the `main` branch
8. **_You should NEVER modify the main branch, we are only merging to it._**
Want to add a feature? Create a separate Issue and a branch for that issue!

