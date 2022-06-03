[chezmoi]: https://github.com/twpayne/chezmoi

# Fork Info

This repository is a fork of [chezmoi] that exists solely to hold branches associated with pull requests.
As such, I do not mirror chezmoi branches other than those associated with pull requests.

## Creating a fork with this method

First, fork the repository on GitHub which is a requirement to make pull requests to upstream.
Then, run the following commands:

```sh
# Create a blank repository that we will use to start fresh.
mkdir chezmoi
cd chezmoi
git init -b forkinfo

# For SSH users:
git remote add origin "git@github.com:YOUR_USERNAME/chezmoi"
# For access token users:
git remote add origin "https://github.com/YOUR_USERNAME/chezmoi"

# Mirror all upstream data to your local repository.
git remote add -f upstream "https://github.com/twpayne/chezmoi"

# Copy LICENSE from upstream as GitHub determines license from default branch.
git checkout upstream/master LICENSE
# Before continuing, create a README.md (feel free to copy this one).

# Now, commit and push this branch to GitHub.
git add .
git commit -m "Initialize fork repository"
git push --set-upstream origin forkinfo
# Before continuing, go on GitHub and change the default branch to 'forkinfo'.

# Delete all branches and tags other than those you checked out locally. If you
# followed these instructions correctly, this should delete everything other than
# the forkinfo branch you just created.
git push --prune --all origin
git tag -l | xargs -n 1000 git push --delete origin --
```

Note that if you had already checked out a branch tracking upstream, the `git push --prune --all origin` command will not remove those branches from your fork.
Your best bet at this point is to delete them from your fork one-by-one.

## Cloning a fork created with this method

```sh
# For SSH users:
git clone "git@github.com:YOUR_USERNAME/chezmoi"
# For access token users:
git clone "https://github.com/YOUR_USERNAME/chezmoi"

# Mirror all upstream data to your local repository.
git remote add -f upstream "https://github.com/twpayne/chezmoi"
```

## Syncing with upstream

```sh
git fetch --all
```

## Creating your own branches

The following command creates a new branch named `my-feature` based on the contents of the upstream branch `master`.
The `--no-track` option is important as Git would otherwise attempt to set the upstream of your new branch to `upstream/master`, when you actually want to push to `origin/my-feature`.

```sh
git checkout --no-track -b my-feature upstream/master
```

## Pushing your branch to your repository

The following command will push branch `my-feature` to your fork of the repository.
If you have previously pushed your currently checked out branch, you can execute a simple `git push`.

```sh
git push --set-upstream origin my-feature
```

## Rebasing your branches

Regularly rebasing your branches until the pull request is merged is important to keep your pull request is up-to-date and free of merge conflicts.
For most Git repositories, it is a bad idea to force push rebased changes, but for ephemeral branches like pull requests it will work fine and make the commit history much cleaner.

```sh
# Make sure you run `git fetch --all` so your tracking refs are up-to-date!
git rebase upstream/master

# If you had previously pushed your changes, make sure to force-push this time.
git push --force
```
