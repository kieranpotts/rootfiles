# Soure Control Workflow

The fork-and-branch workflow is used to manage changes to this project's source code. This allows individual contributors to work in isolation without interfering with anyone else's work.

The steps are as follows:

1. ## Fork

   From this project's GitHub page, click "Fork". The project's repository will be cloned in your own GitHub account. The fork is your "origin" repository, while the main project repository is referred to as the "upstream" repository.

2. ## Clone

   Download a copy of your origin repository:

   ```
   git clone https://github.com/[your-github-username]/github-plugin-theme-contour.git
   ```

   The copy of the project that now exists on your computer is your "local" repository. This is where you'll do your work. 

   When you run the ``git clone`` command, Git automatically adds a remote named "origin", which you will push back to when you've made changes in your local repository. You should add another remote that references the upstream repository. From the root directory of your newly cloned local repository, run the following command in your terminal:

   ```
   git remote add upstream https://github.com/<user>/<project>.git
   ```

3. ## Checkout

   In your local repository, checkout the master ``prod`` branch:

   ```
   git checkout prod
   ```

   If you want to make changes to a previous release, checkout the relevant versioned mainline branch. Example:

   ```
   git checkout prod/v1
   ```

   Pull down the contents of this branch from your origin repository:

   ```
   git fetch origin
   git merge origin
   ```

   These two commands are equivalent to ``git pull origin prod``.

4. ## Branch

   Never make commits directly into a production branch. Instead, branch off from the mainline to create a temporary development branch, where you will make your changes. Please use the following naming convention:

   ```
   dev/<issue-number>-<description>
   ```

   ``<issue-number>`` is the number of the issue that you opened in our issue tracker. ``<description>`` is a concise slug that describes the feature or bug. Example:

   ```
   dev/21-improve-pre-line-height
   ```

   Following this naming convention will help the project maintainers to review and integrate your work later, because each pull request will explicitly reference an open issue.

   Use the following ``git`` commands to create and checkout your development branch:

   ```
   git branch dev/<issue-number>-<description>
   git checkout dev/<issue-number>-<description>
   ```

   Or more succinctly:

   ```
   git checkout -b dev/<issue-number>-<description>
   ```

5. ## Commit

   Undertake your work.

   If you will make substantive changes over a long period, you should make regular commits, organizing your changes in logical iterations. Long-lived development branches should be kept synchronized with the project mainline, and you may like to backup your work by pushing regularly to your origin repository on GitHub. See the next step for instructions.

   Be sure to add or update the relevant test cases, which are in the ``./tests/`` directory, and documentation, which is in the ``./docs/`` directory. Documentation is written using GitHub-Flavoured Markdown.

   Make your changes, and stage and commit them. Using the ``-a`` flag on commit will allow you to submit a commit subject plus a more detailed body message, if you wish.

   ```
   git add <file1> <file2> ...
   git commit -a
   ```

6. ## Synchronize

   If it has been a while since you cloned and checked out the project, you should fetch the latest changes from the appropriate mainline branch in the upstream repository.

   ```
   git checkout prod
   git pull upstream prod
   ```

   Return to your development branch and rebase it on the mainline branch.

   ```
   git checkout dev/<issue-number>-<description>
   git rebase prod
   ```

   As a shortcut, you can rebase the upstream mainline branch directly into your local development branch:

   ```
   git pull --rebase upstream prod
   ```

   The rebasing step will give you the chance to sort out conflicts with the latest work in the project mainline before you request that your work be pulled into the upstream repository. The result is that your PR will have a nice clean diff, so your work can be integrated faster. 

   If you get conflicts during the rebasing process, resolve them, and then continue the rebase.

   ```
   git add <file1> <file2> ...
   git rebase --continue
   ```

   You might like to use interactive rebasing, by adding the ``i`` or ``--interactive`` flag to your ``git rebase`` commands. Interactive rebasing is a powerful feature of the Git version control system. It will pop you into an editing buffer and give you the opportunity to clean up your commit history before subnmitting your work. You can edit and remove individual commits, split them up, reorder them, and even squash them together. For each commit in your local branch you can choose to:

   - **pick**: This is the default behaviour which occurs when you don't do interactive rebasing. It attempts to merge the commit, and will give you the opportunity to resolve any conflicts that Git can't handle itself.
   - **squash**: A squashed commit will have its changes folded into the contents of the preceding commit.
   - **edit**: If you choose this option, the rebasing process will stop and return you to the shell, with the local filesystem tree reflecting the project's state at the selected commit. The index will have the original commit's changes registered for inclusion when you run ``git commit``. You can make further changes before committing, and you can make additional commits if you wish. Run ``git rebase --continue`` when you are finished editing the original commit and to return to the rebasing process.
   - **(drop)**: This option removes a commit. Note, this can cause merge conflicts if any later commits build on the dropped changes.
   
   If all you want to do is squash all of your work into a single commit, you can use the ``--autosquash`` flag instead of doing the squashing yourself using interactive rebasing. 

7. ## Push

   Push the changes in your local development branch to your remote origin repository. Because rebasing changes history, you should use ``-f`` or ``--force`` to force changes into the remote.

   ```
   git push -f origin dev/<issue-number>-<description>
   ```

   If someone else is working on the same branch, use the less destructive ``--force-with-lease``.

8. ## Pull Request

   Now all of your work is in your origin repository, which is attached to your own GitHub user account. From the GitHub page for your origin repository, issue a [pull request](https://help.github.com/articles/about-pull-requests/) to have your work merged in to the project's upstream repository. 

   Go to the Pull Requests section of the upstream repository: 

   https://github.com/<user>/<project>/pulls
   
   Click the "New pull request" button. Click "Compare across forks".
   
   Choose the following for merge:

   - Base repository: ``kieranpotts/github-plugin-theme-contour``
   - Base branch: ``test``
   - Head repository: ``<your-github-username>/github-plugin-theme-contour``
   - Head branch: ``dev/<issue-number>-<description>``

   (Note that pull requests should be made to the ``test`` branch of the upstream repository. All source changes are merged into and tested on this branch. After testing, the project maintainers will release the changes into the production branch — ``prod``.)

   Click the "Create pull request" button.

   Provide a concise but meaningful title for your PR. Use the provided PR template to provide a link back to the original issue and to explain the changes made. Be sure to leave checked the option to "allow edits from maintainers" and click the "Create pull request" button.

   The maintainers of the upstream repository will review and merge your changes into the project.

   By opening a pull request, you agree to this project's [Contributor License Agreement](cla.md).

9. ## Tidy Up

   With everything pushed to the remotes, you can safely delete the temporary development branch from your local repository.

   ```
   git branch -d dev/<issue-number>-<description>
   ```

   If your pull request is accepted, and after your work is merged into the upstream repository, you can delete the development branch from your remote origin repository, too:

   ```
   git push origin :dev/<issue-number>-<description>
   ```
