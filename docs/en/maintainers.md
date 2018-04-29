# Notes for Project Maintainers

The following documentation is intended for use by project maintainers — that's anyone with write privileges to this project's upstream repository.

## Pull Requests

Pull requests are expected to be made into the ``test`` branch from development branches.

If the changes are small and can be handled automatically, the merge can be undertaken via GitHub's interface. But generally it is better to checkout the development branch, merge and test it locally, sorting out any problems with the integration before pushing the changes up to ``test`` on the upstream remote.

Follow these steps.

Checkout a new branch, forked from ``test``. Use the name of the development branch that contains the changes to be reviewed and merged into the project mainline.

```
git checkout -b dev/<issue-number>-<description> test
```

Pull the changes down from the relevant remote repository. This may be a fork of the upstream repository, if the PR is from an external contributor.

```
git pull git://github.com/<user>/<project>.git dev/<issue-number>-<description>
```

Test the changes and, if necessary, introduce new commits to get everything to work as expected.

If you approve of the changes, merge them into the ``test`` branch. Use the ``--no-ff`` flag to ensure that an explicit merge commit is recorded. You can just use the default commit message, it will be something like "Merge branch 'dev/update-authors' into test".

```
git checkout test
git merge --no-ff dev/<issue-number>-<description>
```

Push the changes to the ``test`` branch in the upstream repository:

```
git push origin test
```

On GitHub, the Pull Request should now be marked as "Merged".


## Releasing

When the tip of ``test`` is known to be stable and ready for deployment, a new release can be made. Follow these steps:

First, merge ``prod`` into ``test`` to resolve any conflicts in ``test`` first:

```
git checkout test
git merge prod
```

Resolve any conflicts and run tests. Then merge ``test`` into ``prod``, using the ``--no-ff`` to ensure that the merge is explicitly recorded in the commit history:

```
git checkout prod
git merge --no-ff test
```

Releases are tagged in the ``prod`` branch. We use [Semver](http://semver.org/), the gold standard in software version numbering. Version numbers are written as ``v<major>.<minor>.<patch>``, e.g. ``v1.0.5``.

Update the version number in the following files:

- ``./CHANGELOG.md``
- ``./package.json``

Update the CHANGELOG with a list of functional changes and bug fixes. Use the commit and PR history to gather this information.

Commit all of these changes in a single commit that is dedicated to bumping the version number. The commit message must be in the format ``Preparing v1.0.0``.

Create an annotated tag on the ``prod`` branch: 

```
git tag -a v1.0.0 -m "Tagging v1.0.0"
```

Push the changes and tags:

```
git push --tags origin prod
```
