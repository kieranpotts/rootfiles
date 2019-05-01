# How to Prepare New Releases

The following documentation is intended for reference by the project maintainers — that's anyone with write privileges to this project's main "upstream" repository.

#### Pull Requests

Pull requests are expected to be made into the ``dev`` branch (which is the default) from various ``issue`` branches. Merging PRs into the project mainline is the start of the process of preparing a new release of the software. Avoid merging changes that you do not plan to include in the next release, unless those changes can be disabled via a feature toggle.

If the changes are small and can be handled automatically, the merge may be done via GitHub's interface. But this is not recommended. Generally, it is better to checkout the issue branch, rebase and test it locally, sorting out any problems with the integration before merging it proper and pushing the changes to the upstream repository on GitHub. 

Here's how you do that. (These instructions assume that the upstream remote is referenced as "origin" in your local clone.)

In your local repository, fetch and merge the latest work from the upstream ``dev`` branch.

```sh
$ git checkout dev
$ git pull origin dev
```

Checkout a new branch, based on ``dev``. Use the name of the ``issue`` branch that contains the changes that you want to pull in and review.

```sh
$ git checkout -b issue-[number]/[description] dev
```

Pull in the work to be reviewed from the relevant remote repository. This may be a fork of the upstream repository — rather than "origin" — if the PR is from an external contributor.

```sh
$ git pull git://github.com/[username]/[repo].git issue-[number]/[description]
```

Rebase the issue branch on ``dev``.

```sh
$ git rebase dev
```

Resolve any conflicts with work recently added to the project. Test the changes and, if necessary, introduce new commits to get everything working as expected. Review associated tests and documentation.

If you approve of the changes, and you wish to include them in the next release, update the CHANGELOG, adding the change to the "unreleased" version at the top of the file. 

Next, merge the issue branch into the ``dev`` branch. Use the ``--no-ff`` flag to ensure that an explicit merge commit is recorded. The default commit message, which will be something like "Merge branch issue-45/fix-something into dev", will do just fine.

```sh
$ git checkout dev
$ git merge --no-ff issue-[number]/[description]
```

Push the changes to the ``dev`` branch in the upstream repository.

```sh
$ git push origin dev
```

On GitHub, the Pull Request may have been automatically marked as "Merged". If not, go ahead and close the PR, leaving a comment that links to the merge commit. You can also close the associated issue if it has not been closed automatically.

You can now delete your local copy of the issue branch.

```sh
$ git branch -d issue-[issue]/[description]
```

#### Releases

The ``dev`` branch may be unstable or incomplete from time to time, when changes are in the process of being merged into the mainline. Project maintainers must undertake the following tasks to verify that the ``dev`` branch is complete and stable, before preparing a new release:

* Merge ``qa`` into ``dev``, to capture any other work that was recently merged into the mainline, and resolve any conflicts
* With all dependencies up-to-date, rebuild the distribution software (e.g. ``npm run build``)
* Review all tests and demos
* Run through any other pre-release test scripts

When the tip of the ``dev`` branch is known to be stable and deployable, a new version of the software can be released.

Final quality assurance checks are undertaken and new releases are prepared in the ``qa`` branch. Make sure your local ``qa`` branch is up-to-date with the ``qa`` branch on the remote, then merge ``dev`` into ``qa``.

```sh
$ git checkout qa
$ git pull origin qa

$ git merge dev
```

Note, the difference between the ``dev`` and ``qa`` branches are that ``dev`` may at times be unstable. The latest commits in ``dev`` should only be merged into ``qa`` when the ``dev`` branch is proven to be stable.

Review the CHANGELOG. The "unreleased" version of the software should already list all of the functional changes and bug fixes. Double check that all changes are included by reviewing the commit history since the last release.

Now it's time to decide what the version number of the next release should be. We use [Semver](http://semver.org/), the gold standard in software version numbering. Bump the version number in the following files:

- ``./CHANGELOG.md``
- ``./package.json``

Apply these changes in a single commit that is dedicated to bumping the version number. The commit message must be in the format ``Preparing v[major].[minor].[patch]``. Example:

```sh
$ git add .
$ git commit -m "Preparing v2.3.11"
```

Push your local changes to the remote ``qa`` branch.

```sh
$ git push origin qa
```

Merge ``qa`` into ``prod``. This time, use  the ``--squash`` flag to squash all changes since the previous release into a single commit. This means that the history of the ``prod`` branch uniquely will reflect the release history, rather than the more granular commit history (which is recorded in ``dev`` and ``qa``).

```sh
$ git checkout prod
$ git merge --squash qa
```

All of the changes since the previous release will now be "staged" in your local repository. Add the lot to the ``prod`` branch in a single commit:

```sh
$ git commit -m "v2.3.11"
```

On the ``prod`` branch, against the last commit, create an annotated tag.

```sh
$ git tag -a v2.3.11 -m "Tagging v2.3.11"
```

Push the changes, including the new tags, to the ``prod`` branch in the origin repository.

```sh
$ git push --tags origin prod
```

From Github's index page for the upstream repository, go to "Releases" and click "Draft a new release". Select the new version tag on the ``prod`` branch. The release title should match the version number, e.g. "v2.3.11". For the short summary, copy the release notes from the CHANGELOG. Finally, click "Publish release".

That's it. A new version of the software has been formally released.

There's one final bit of housekeeping to do. Please make sure that ``dev`` captures any commits made directly to the ``qa`` branch during the release process. At the very least, this will give developers an up-to-date CHANGELOG.  

```sh
$ git checkout dev
$ git merge qa
```

You should not get conflicts, but if you do, fix them and save the changes, then run the following commands:

```sh
$ git add .
$ git commit -m "Merge branch qa into dev"
$ git push origin dev
```

There is no need also to merge the ``prod`` branch back to ``dev``, unless you made commits directly to this branch during the release process.
