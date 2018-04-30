# Notes for Project Maintainers

The following documentation is intended for use by project maintainers — that's anyone with write privileges to this project's main ("upstream") repository.

## Pull Requests

Pull requests are expected to be made into the ``test`` branch from various ``dev/**`` branches.

If the changes are small and can be handled automatically, the merge can be undertaken via GitHub's interface. But generally it is better to checkout the development branch, merge and test it locally, sorting out any problems with the integration before pushing the changes back to the upstream repository on GitHub. 

The following instructions assume that the upstream remote is referenced as "origin" in your local clone.

In your local repository, fetch and merge the latest works in the ``test`` branch.

```
git checkout test
git pull origin test
```

Checkout a new branch, based on ``test``. Use the name of the development branch that contains the changes that you want to review.

```
git checkout -b dev/[issue]-[description] test
```

Pull in the work to be reviewed from the relevant remote repository. This may be a fork of the upstream repository — rather than "origin" — if the PR is from an external contributor.

```
git pull git://github.com/[user]/[project].git dev/[issue]-[description]
```

Test the changes and, if necessary, introduce new commits to get everything working as expected. Review associated tests and documentation.

If you approve of the changes, merge them into the ``test`` branch. Use the ``--no-ff`` flag to ensure that an explicit merge commit is recorded. The default commit message, which will be something like "Merge branch dev/45-fix-something into test", will do just fine.

```
git checkout test
git merge --no-ff dev/[issue]-[description]
```

Push the changes to the ``test`` branch in the upstream repository.

```
git push origin test
```

On GitHub, the Pull Request should now be marked as "Merged". You can delete your local copy of the development branch.

```
git branch -d dev/[issue]-[description]
```

## Releases

When the HEAD commit in the ``test`` branch is known to be stable and deployable, a new version of the software can be released.

First, do a reverse merge, merging ``prod`` backwards into ``test``.

```
git checkout test
git merge prod
```

Resolve any conflicts in the ``test`` branch. Run automated tests and work through any pre-release test scripts to confirm that the HEAD commit in your local ``test`` branch is stable.

Releases are tagged in the ``test`` branch before ``test`` is merged into ``prod``. We use [Semver](http://semver.org/), the gold standard in software version numbering. Version numbers are written as ``v[major].[minor].[patch]``, e.g. ``v1.0.5``. Bump the version number in the following files:

- ``./CHANGELOG.md``
- ``./package.json``

Update the CHANGELOG with a list of functional changes and bug fixes. Use the commit and PR history to gather this information.

Apply all of these changes in a single commit that is dedicated to bumping the version number. The commit message must be in the format ``Preparing v1.0.0``.

```
git commit -m "Preparing v1.0.0"
```

Create an annotated tag.

```
git tag -a v1.0.0 -m "Tagging v1.0.0"
```

Push the changes, including the tags, to the ``test`` branch in the upstream repository.

```
git push --tags origin test
```

To complete the release, merge ``test`` into ``prod``. Use  the ``--no-ff`` to ensure that the merge is explicitly recorded in the commit history.

```
git checkout prod
git merge --no-ff test
```

Push the changes, including the new tags, to the ``prod`` branch in the upstream repository.

```
git push --tags origin prod
```
