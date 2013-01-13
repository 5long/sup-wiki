# Contributing to Sup

Sup is versioned with Git. This makes it very easy to contribute
code changes and bug fixes. The typical lifecycle of a contributed
change looks something like this:

# You develop locally
# You submit patch to mailing list
# You endure public commentary
# You revise and go to \#2, if necessary
# Maintainer merges changes into 'next' branch
# Users tracking next try out your changes and report problems
# You submit bugfix patches and go to \#2, if necessary
# Maintainer merges cumulative changes into 'master' branch
# Maintainer cuts a release from master, including your changes

Git has several tools that make this workflow easy. The following
is a brief intro on how to use Git when developing Sup. It is meant
as a quick-start and is by no means a complete guide to Git. If
you're going to be doing significant development, you will
definitely need to learn more about Git than is presented here.

## Branches

In the git world, branches are ubiquitous. They come it two
flavors: remote branches, which you can pull changes from (or push
to), and local branches, which you can edit. To use a remote
branch, you have to make a local branch that mirrors it.

From this lifecycle above, you can see that the Sup repository has
two remote branches: master and next. Master is as stable as
possible. Next is where the latest and greatest features go. Users
are encouraged to track both.

Once changes mature, they're pulled from next to master, and
releases are cut from master. Simple and obvious bug-fix patches
can also be applied directly to master, in which case master is
merged into next to propagate those changes.

If you're interested in trying out the new fancy features, you will
want to track next.

If you're interested in the most stable version possible, you will
want to track master.

If you're interested in developing, you will typically develop
against master.

## Setting up

First, you need to clone the sup repository.

      git clone git://gitorious.org/sup/mainline.git sup

This will create a "sup" directory, which contains everything you
need to get started. (In fact, it contains a complete copy of the
entire development history!)

By default you're on the master branch.

## Running from the "next" branch

To get the latest and greatest features, you need to switch to the
next branch. The Git way to do this is like so:

      git branch --track next origin/next
      git checkout next

The first command creates a local branch called next, which tracks
the remote branch, origin/next. Tracking means that we can use
commands like git pull to keep this branch up to date with the
remote repository.

The second command switches you over to that branch. You can switch
back to the master branch with a git checkout master at any point.

## Keeping up to date

At any point, you can receive the most recent version of the
current branch with a git pull:

      git pull

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

## Bugfix changes against next

If you encounter a bug in next, and want to submit a patch, then
clearly you can't work off of master. It's fine to submit patches
off of next. For maximum likelihood of patch acceptance, find the
commit that created the bug, create a branch right there with git
branch `<branchname\> <commit id\>`, and do your bugfix commits on
that branch. That way, the patches don't have false dependencies on
later features of next.



## Keeping up to date while developing

If changes have been made to master since you started working,
instead of merging, we will rebase. Rebasing essentially moves the
branch point to the new head of master, as if you've been
developing there all along, by rewinding and replaying them from
the new head. (Conflict resolution works the same as with merging.)
This keeps your development history clean and simple.

To rebase your topic branch:

     git checkout master       # switch to master
     git pull                  # sync master branch with remote repository
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

## Submitting patches for review

Once you've committed some code, it's time to submit it to the
mailing list for review. You can use git-format-patch to create one
email per commit, which you can then send with Sup, or, if you're
feeling courageous, you can use git-send-email to create the
patches and send them in one fell swoop.

Typical usage will be something like:

      git-format-patch master # make patch files for all commits since branching from master

If you've structured your commit messages as above, the summary
will be used as the subject (after a "[PATCH]" marker), and the
descriptive version and the diff itself will appear in the body of
the message.

On the maintainer's end, there are some nice tools to import emails
in the format as commits, so submitting your patches in this manner
makes his life easy.

There are some good tips for how to submit beautiful patches on the
[Git project's SubmittingPatches document](http://repo.or.cz/w/git.git?a=blob;f=Documentation/SubmittingPatches;hb=HEAD).
In particular, if you want to add explanatory/"cover letter" text,
you can place it after the --- but before the diffstat in the
email.

(They require the Signed-off-by: field to indicate some legal
stuff. Sup hasn't become quite so bureaucratic yet, so I will
merely assume that, by submitting patches, you are aware that I may
very well use and distribute them in Sup according to the Sup
license.)



## Other useful commands

Other useful commands include gitk --all for visualizing the entire
development history, git reflog for getting yourself out of a mess
you made with git reset or git merge, and git rebase --onto for
serious graph editing.

## The end

Good luck, and happy patching!
