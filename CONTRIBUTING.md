# How to Contribute

This is a free and open source software project. It is made possible by generous contributions from software engineers, both expert and novice, from around the world. There are three ways that you can get involved:

- Request features
- Report bugs
- Contribute source code, documentation and tests

All contributions – bug reports, feature requests, source changes – are managed via the project's [issue tracker](https://github.com/username/project/issues). By participating in this project you agree to abide by the terms of this project's Contributor Code of Conduct, which is set out below.


## Contributor Code of Conduct

The maintainers of this project pledge to respect all people who contribute through reporting issues, posting feature requests, updating documentation, submitting pull requests or patches, and other activities.

We are committed to making participation in this project a harassment-free experience for everyone, regardless of level of experience, gender, gender identity and expression, sexual orientation, disability, personal appearance, body size, race, ethnicity, age, or religion.

Examples of unacceptable behavior by participants include the use of sexual language or imagery, derogatory comments or personal attacks, trolling, public or private harassment, insults, or other unprofessional conduct.

Project maintainers have the right and responsibility to remove, edit, or reject comments, commits, code, wiki edits, issues, and other contributions that are not aligned to this Code of Conduct. Project maintainers who do not follow the Code of Conduct may be removed from the project team.

This Code of Conduct applies both within project spaces and in public spaces when an individual is representing the project or its community.

Instances of abusive, harassing, or otherwise unacceptable behavior may be reported by opening an issue or contacting one or more of the project maintainers directly.

This Code of Conduct is adapted from the [Contributor Covenant](https://www.contributor-covenant.org), version 1.1.0, available at https://www.contributor-covenant.org/version/1/1/0/code-of-conduct.html.


## Feature Requests

Feature requests are welcome. Feature requests may be submitted via the [issue tracker](https://github.com/username/project/issues). Please add the "enhancement" label and prefix the title "Idea:".

Take a moment to consider whether your idea fits with the scope and aims of the project, and what are the benefits versus costs of adding the feature.

Feature requests will remain open in the issue tracker until they are approved by the project maintainers, who will be influenced by :+1: reactions and other comments by the community.


## Bug Reports

Before reporting a bug, please check that it has not already been reported previously by searching the [issue tracker](https://github.com/username/project/issues). Also check that the problem has not already been fixed by trying to reproduce the problem in the ``master`` branch.

To fix an error, the project maintainers must be able to reproduce it. This requires you to be able to demonstrate the bug. A good bug report will therefore include at least one of the following:

- Detailed step-by-step instructions to reproduce the error.
- A live example of the problem, e.g. a [Codepen](http://codepen.io/).
- A screenshot if reporting a visual regression.
- A failing test case.

If relevant, please provide details of the browser name, version number, and operating system in which you experienced the error. Describe the outcome that you expected, and what's different about the actual outcome.

If you can identify the source of the bug, such as a specific line or block of code, please include this information in your bug report, too.


## Source Changes

The ultimate way to contribute to this project (indeed, to _any_ open source software project) is to contribute material changes to its source code, tests, and documentation. This is done using the pull request (PR) system.

**Please raise an issue before making a pull request.** Make a note that you would be willing to have the issue assigned to you to resolve. The project maintainers will review your bug report or feature request, and they will accept or reject it before you spend time implementing the fix or enhancement.

The fork-and-branch workflow is used to manage changes to this project's source code. This allows individual contributors to work in isolation without interfering with anyone else's work.

The steps are as follows:

### 1. Fork

From this project's GitHub page, click "Fork". The project's repository will be cloned in your own GitHub account. The fork is your "origin" repository, while the main project repository is referred to as the "upstream" repository.

### 2. Clone

Download a copy of your origin repository:

```
git clone https://github.com/your-github-username/project-name.git
```

The copy of the project that now exists on your computer is your "local" repository. This is where you'll do your work. 

When you run the ``git clone`` command, Git automatically adds a remote named "origin", which you will push back to when you've made changes in your local repository. You should add another remote that references the upstream repository. From the root directory of your newly cloned local repository, run the following command in your terminal:

```
git remote add upstream https://github.com/username/project-name.git
```

### 3. Checkout

In your local repository, checkout the ``master`` branch:

```
git checkout master
```

If you want to make changes to a previous release, checkout the relevant versioned mainline branch. Example:

```
git checkout master/v1
```

Pull down the contents of this branch from your origin repository:

```
git fetch origin
git merge origin
```

These two commands are equivalent to ``git pull origin master``.

### 4. Branch

Never make commits directly into a master branch. Instead, branch off from the mainline to create a temporary branch, where you will make your changes. Please use the following naming convention:

```
issue/<issue-number>-<description>
```

``<issue-number>`` is the number of the issue that you opened in our issue tracker. ``<description>`` is a concise slug that describes the feature or bug. Example:

```
issue/21-improve-pre-line-height
```

Following this naming convention will help the project maintainers to review and integrate your work later, because each pull request will explicitly reference an open issue.

Use the following ``git`` commands to create and checkout your issue branch:

```
git branch issue/21-improve-pre-line-height
git checkout issue/21-improve-pre-line-height
```

Or more succinctly:

```
git checkout -b issue/21-improve-pre-line-height
```

### 5. Commit

Undertake your work.

If you are making substantive changes over a long period, you should make regular commits, organizing your changes in logical iterations. Long-lived issue branches should be kept synchronized with the project mainline, and you may like to backup your work by pushing regularly to your origin repository on GitHub. See below for instructions.

Be sure to add or update the relevant test cases, which are in the ``./tests/`` directory, and documentation, which is in the ``./docs/`` directory. Documentation is written using GitHub-Flavaoured Markdown.

Make your changes, and stage and commit them. Using the ``-a`` flag on commit will allow you to submit a commit subject plus a more detailed body message, if you wish.

```
git add <file1> <file2> ...
git commit -a
```

### 6. Synchronize

If it has been a while since you cloned and checked out the project, you should fetch the latest changes from the appropriate mainline branch in the upstream repository.

```
git checkout master
git pull upstream master
```

Return to your issue branch and [rebase](https://help.github.com/articles/about-git-rebase/) it on the mainline branch.

```
git checkout issue/21-improve-pre-line-height
git rebase -i master
```

As a shortcut, you can rebase the upstream mainline branch directly into your local issue branch:

```
git pull --rebase -i upstream master
```

The rebasing step will give you the chance to sort out conflicts with the latest work in the project mainline before you request that your work be pulled into the upstream repository. The result is that your PR will have a nice clean diff, so your work can be integrated faster. 

Interactive rebasing, using the ``-i`` or ``-interactive`` flag, is recommended. It will allow you to clean up your commit history before submitting your work. You can edit old commits, split them up, reorder them, and even squash some. If you have a particularly messy commit history, you may choose to use ``--autosquash`` to consolidate all of your work into a single commit. 

If you have conflicts, resolve them, and then continue the rebase.

```
git add <file1> <file2> ...
git rebase --continue
```

### 7. Push

Push the changes in your local issue branch to your remote origin repository. Because rebasing changes history, you should use ``-f`` or ``--force`` to force changes into the remote.

```
git push -f origin issue/21-improve-pre-line-height
```

If someone else is working on the same branch, use the less destructive ``--force-with-lease``.

### 8. Pull request

Now all your work is in your origin repository, which is attached to your GitHub user account. From the GitHub page for your origin repository, issue a [pull request](https://help.github.com/articles/about-pull-requests/) to have your work merged in to the project's upstream repository. Click the "Pull Request" button and follow the steps. Provide a clear title and description, and be sure to leave checked the option to "allow edits from maintainers". 

The maintainers of the upstream repository will review and merge your changes into the project.

By opening a pull request, you agree to our Contributor License Agreement - see below.

### 9. Tidy up

With everything pushed to the remotes, you can safely delete the temporary issue branch from your local repository.

```
git branch -d issue/21-improve-pre-line-height
```


## Contributor License Agreement

By issuing a pull request (PR) to this project's GitHub repository, you accept and agree to the following terms and conditions regarding your contribution - which may include, without limitation, software, bug fixes, configuration changes, documentation, test cases, and any other materials.

You agree that your contribution may be distributed under the terms of the [MIT License](https://opensource.org/licenses/MIT), with the exception of project documentation that you agree may be distributed under the terms of the [Creative Commons Attribution 4.0 International (CC BY 4.0) License](https://creativecommons.org/licenses/by/4.0/legalcode).

You certify that your contribution is either: created in whole by you and you have the right to submit it under the designated license; or based upon previous work that to the best of your knowledge is covered under an appropriate open source software license amd you have the right under that license to submit the work with modifications under the designated license. 

You understand and agree that your contribution is public and that a record of the contribution, including all metadata and personal information you submit with it, is maintained indefinitely and may be redistributed as per the requirements of the designated licenses.


## Notes for Project Maintainers

### Pull Requests

Pull requests are expected to be made into one of the ``master`` from branches with the following naming convention and corresponding to an open issue:

```
issue/<issue-number>-<description>
```

Do not use GitHub's merge option. Instead, checkout the whole branch and test the changes locally, sorting out any problems with the integration before merging into a mainline branch and pushing to the remote repository.

### Release Checklist

- Update the version number in the following files:
  - xxxxx
  - xxxxx
  - xxxxx
- Use a dedicated commit to bump the version number. The commit message must be in the format ``v1.0.0``. In the same commit, update the CHANGELOG with a list of functional changes.
- Create an annotated tag on the appropriate ``master`` branch: ``git tag -m "v1.0.0" v1.0.0``.
- Push the changes and tags: ``git push --tags origin master``.
- Update the website by checking out the ``gh-pages`` branch and following the instructions in the README in that branch.

[Semver](http://semver.org/) is the gold standard for software version numbering. Version numbers are written as ``v<major>.<minor>.<patch>``, e.g. ``v1.0.5``.

