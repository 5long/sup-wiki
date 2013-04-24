# Contributing to Sup

Sup is versioned with Git. This makes it very easy to contribute
code changes and bug fixes. The typical lifecycle of a contributed
change looks something like this:

1. You fork the repository on github
2. You clone your fork
3. You develop locally on a feature branch
4. You push your feature branch to your fork on github
5. You start a pull request from your feature branch to the develop branch on the upstream repo
6. You endure public commentary
7. You revise and go to \#5, if necessary
8. A maintainer merges the pull request into the develop branch
9. Users tracking 'develop' try out your changes and report problems
10. You submit bugfix patches and go to \#5, if necessary

Then every now and then:

1. Maintainer merges cumulative changes into 'master' branch
2. Maintainer cuts a release from master, including your changes

Git has several tools that make this workflow easy. The following
is a brief intro on how to use Git when developing Sup. It is meant
as a quick-start and is by no means a complete guide to Git. If
you're going to be doing significant development, you will
definitely need to learn more about Git than is presented here.

## Branches

In the git world, branches are ubiquitous. They come in two
flavors: remote branches, which you can pull changes from (or push
to), and local branches, which you can edit. To use a remote
branch, you have to make a local branch that mirrors it.

From this lifecycle above, you can see that the Sup repository has
two remote branches: master and develop. Master is as stable as
possible. develop is where the latest and greatest features go. Users
are encouraged to track both.

Once changes mature, they're pulled from develop to master, and
releases are cut from master. Simple and obvious bug-fix patches
can also be applied directly to master, in which case master is
merged into next to propagate those changes.

If you're interested in trying out the new fancy features, you will
want to track develop.

If you're interested in the most stable version possible, you will
want to track master.

If you're interested in developing, you will typically develop
against master.

## Setting up

First you need to fork the sup repository.  Log in to https://github.com
and go to https://github.com/sup-heliotrope/sup.git and click on the
"Fork" button.

Now, you need to clone your fork of the sup repository.

      git clone https://github.com/<username>/sup.git

This will create a "sup" directory, which contains everything you
need to get started. (In fact, it contains a complete copy of the
entire development history!)

By default you're on the master branch.

## Running from the "develop" branch

To get the latest and greatest features, you need to switch to the
develop branch. The Git way to do this is like so:

      git branch --track develop origin/develop
      git checkout develop

The first command creates a local branch called develop, which tracks
the remote branch, origin/develop. Tracking means that we can use
commands like git pull to keep this branch up to date with the
remote repository.

The second command switches you over to that branch. You can switch
back to the master branch with a git checkout master at any point.

## Keeping up to date

In order to pull in the latest changes from upstream you will need to
set up a git remote that points to the upstream repo:

    git remote add upstream git://github.com/sup-heliotrope/sup.git

At any point, you can receive the most recent version of the
current branch with a git pull:

    git pull upstream

If you've made uncommitted changes locally directly on this branch,
this command will probably fail. If you've made changes locally
directly on this branch, and committed them, this command will do a
merge. In both cases, that probably isn't what you want. Follow the
instructions in the next section instead.

## Developing

To develop a particular feature, Git works best with the "topic
branch" methodology --- each feature gets its own branch and is
developed independently of everything else. At the end of the day,
these branches are merged in to the mainline branches.

New features should be developed off master. Here's how to get
started developing a feature:

     git checkout master       # switch to master barnch
     git branch <topic name>   # create a new branch off of here
     git checkout <topic name> # switch to the new branch

These commands can be combined into one: `git checkout -b <topic name\> master`

To do the first push to your fork, and to set up your local copy to
track the branch on your fork, do:

    git push -u origin <topic name>

Now you're on the topic branch, and you can tweak and commit to
your heart's content. Committing code is a two-stage process.
First, you use git add to select the changes you want to commit.
(Hint: `git add -p` in very recent Gits, will walk you through on a
hunk-by-hunk basis, which is very nice, and you can use
[git-wt-add](http://git-wt-commit.rubyforge.org/) if your git
doesn't have this yet). This is called staging the changes, and you
can see what's been staged with `git diff --cached`

Then, use git commit to commit all staged changes. (Hint: `git commit -v` will display the diff in the commit message file.)

*Important*: lots of Git tools rely on a particular format of the
commit message, but this isn't really explicitly stated anywhere.
The format is:

      <one line lowercased summary of the commit, probably less than 72 chars>
      <blank line>
      <more descriptive stuff>

Get in the habit of writing your commit messages like this. It will
make your life easier later down the road. (And when you submit
patches to the mailing list, the patches will include the commit
messages, and they will be rejected unless they conform to this
format.)

Other useful commands for developing locally: `git log` (try -p) and
`git stash`

Commit early and often. It's all done locally. Before submitting
our code to the outside world, we'll have a chance to clean it up
by splitting, merging, dropping commits, and editing commit
messages.

## Bugfix changes against develop

If you encounter a bug in develop, and want to submit a pull request, then
clearly you can't work off of master. It's fine to submit pull requests
off of develop. For maximum likelihood of patch acceptance, find the
commit that created the bug, create a branch right there with git
branch `<branchname\> <commit id\>`, and do your bugfix commits on
that branch. That way, the patches don't have false dependencies on
later features of develop.

## Keeping up to date while developing

If changes have been made to master since you started working,
instead of merging, we will rebase. Rebasing essentially moves the
branch point to the new head of master, as if you've been
developing there all along, by rewinding and replaying them from
the new head. (Conflict resolution works the same as with merging.)
This keeps your development history clean and simple.

To rebase your topic branch:

     git checkout master       # switch to master
     git pull upstream master  # sync master branch with remote repository
     git checkout <topic name> # switch back
     git rebase master         # move entire branch up

## Preparing to submit changes

Once you've developed and tested locally on your topic branch,
you're almost ready to submit your patches to the mailing list.
First you should spend some time reviewing your commits, e.g. with
git log -p, making sure:

* They're grouped into logical units
* They don't introduce extraneous whitespace (use git diff --check, or git log -p with color turned on)
* The commit messages are formatted properly (see above)

Your biggest tool at this stage is git rebase --interactive, which
will help you edit and reorder your commits until they're ready for
public exposure. You will definitely need to read the man page, but
typical usage will be something like:

     git rebase --interactive master # edit all commits since branching from master

## Submitting pull requests for review

We prefer to get contributions via pull requests on github.  To do this
you'll need to push your topic branch to your fork:

    git checkout <topic name>
    git push

Then you can login to github and go to your fork page at
`https://github.com/<username>/sup` and github will notice that you
recently pushed a branch and offer to start a pull request.  Click on
that button, enter any text you need to explain what your pull request
is about and click the Submit button.

There are some good tips for how to submit beautiful patches on the
[Git project's SubmittingPatches document](http://repo.or.cz/w/git.git?a=blob;f=Documentation/SubmittingPatches;hb=HEAD).
In particular, if you want to add explanatory/"cover letter" text,
you can place it after the --- but before the diffstat in the
email.


## Other useful commands

Other useful commands include gitk --all for visualizing the entire
development history, git reflog for getting yourself out of a mess
you made with git reset or git merge, and git rebase --onto for
serious graph editing.

## The end

Good luck, and happy patching!
