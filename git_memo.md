### Typical rebase workflow

Given the following:
* `develop` is your 'mainline' local branch
* `origin` is your remote (you can verify this by typing `git remote -v`)
* Your local branch `develop` tracks the remote branch `develop`,
  also known as `origin/develop`

Start by checking out `develop`, then create a new branch `some_feature`
from it:

    git checkout develop
    git checkout -b some_feature

Do work in your branch `some_feature` and commit your changes.
By now, the remote `develop` branch may have been updated with
new changes by other contributors.

While you are on the branch `some_feature`, fetch those changes,
merge them into `some_feature`, and place your work on top
(this is called *rebasing*):

    git fetch origin
    git rebase origin/develop

Once you finish working on your feature, rebase one last time to
ensure that `some_feature` contains the most recent commits from
`origin/develop` (as shown above), then merge `some_feature` back
into your mainline branch `develop`:

    git checkout develop
    git merge --ff-only some_feature

Finally, you can delete the branch `some_feature` since you already
merged it into `develop`:

    git branch -d some_feature


### Push changes to Gerrit

The following pushes the contents of the current branch to Gerrit,
targeting the branch `develop`.

    git push origin HEAD:refs/for/develop


### Push commits from one branch to another

    git push <remote> <source branch>:<destination branch>

Note that this will only work if it's a fast-forward of the destination
branch.


### Revise the most recent commit

After making changes:

    git commit --amend

You can change the commit message if you want, but keep the original Change-Id.


### Squash commits with interactive rebase

**Note**: Only use this for commits that haven't been pushed to the remote
repository.

The following lets us view the last `N` commits (including the current
commit that `HEAD` is referencing), and make adjustments to them.

    git rebase -i HEAD~N

Keep the original Change-Id.


### Push a local branch to the repository

    git push remote-name branch-name

or

    git push remote-name local-branch-name:remote-branch-name

Where `remote-name` is typically `origin`. For example,

    git push origin HEAD:refs/for/develop

To create an upstream branch so a subsequent `git pull` will know
that the local branch `foo` is linked to the remote branch we are about
to create, `origin/foo`:

    git push -u remote-name branch-name

(or following our example, `git push -u origin foo`)


### Find out which remote branch a local branch is tracking

    git branch -vv

or

    git remote show origin


### Create a new branch and have it track a remote branch of the same name

    git checkout -b test remote-name/test

or

    git checkout test


### Check out a specific commit to a new branch

    git checkout -b branch-name revision-hash


### Review a patch set from Gerrit

1. Go to the change using the Gerrit web UI
2. At the top right, select the patch set you want to review
   by using the "Patch Sets (`x`/`y`)" button. `x` is the number
   of the patch set you want to review, and `y` is the total number
   of patch sets.
3. Next, click the "Download" button to the right of the
   "Patch Sets (`x`/`y`)" button.
4. Copy and run the command line command for "Checkout" which should look
   something like this:
   ```git fetch ssh://user@domain.com:9999/project refs/changes/66/190366/4 && git checkout FETCH_HEAD```.
   This will put you in a "detached HEAD" state.
5. Then create a new branch for the patch set
       git checkout -b 190366-4


### Delete a branch locally

If the branch has already been merged into your mainline branch:

    git branch -d branch-name

Otherwise:

    git branch -D branch-name


### Set a local existing branch to track a remote branch

The following works as of Git 1.8.0. Given a local branch `foo`
and a remote `origin`:

If you are already on the branch `foo`:

    git branch -u origin/foo

If you are not on the branch `foo` but would like `foo`
to track `origin/foo`:

    git branch -u origin/foo foo


### Configure git to use Sublime Text as its default editor

First, make a symbolic link to the Sublime Text editor
(assuming that you want to store the symbolic link under ~/bin):

    ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl ~/bin/subl

If necessary, add `~/bin` to your `PATH` variable by adding
the following line to your `~/.bash_profile`:

    export PATH="$PATH:~/bin"

Then `source ~/.bash_profile` to reload your changed `.bash_profile`.
Now test `subl` to make sure it works.

Configure git to use `subl` as the default text editor:

    git config --global core.editor "subl -n -w"

Now running `git commit` will launch Sublime Text in a new window (`-n`)
with a file for your commit message, and git will wait until you have
saved and closed the file containing your commit message (`-w`).
Note that the `-w` switch is necessary for Sublime Text to interact with
git properly; it makes Sublime Text wait for the file to be closed before
returning.


### Show a compact view of git log with full hashes of commits

    git log --pretty=oneline


### Show a log of all commits from all branches

    git log --all --graph --decorate --pretty=oneline


### View the evolution of a range of lines in a file

    git log --pretty=short -u -L start-line,end-line:file


### View a particular version of a file

    git show revision-hash:path/to/file

**Note**: This opens up the file in `less` without line numbers.
To toggle line numbers on within `less`, type `-N`.


### Check where HEAD is pointing to

In the root directory of the repository, run

    cat .git/HEAD


### Undo the last commit

    git reset --soft HEAD~1


### Stage all changes of a branch `some-branch` into the current branch

    git merge --no-commit --squash some-branch


### Abort a merge that encountered conflicts

Works for versions of Git >= 1.6.1

    git reset --merge


### Diff specific versions of a file

Compare the current (the version in your working tree) `foo.txt` with
the `foo.txt` at `HEAD`:

    git diff HEAD:foo.txt foo.txt

The general syntax is `revision:filename`. There are many ways of specifying
a revision (e.g. SHA-1 hash), which you can use in the above example instead
of `HEAD`


### Diff the tips of two branches

Show what's in b2 that's not in b1:

    git diff b1 b2

or

    git diff b1..b2

Show changes that occurred on b2 since b1 was started off of it (i.e. their common ancestor):

    git diff b1...b2
