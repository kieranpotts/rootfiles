# Soure Control Workflow

Please use the following fork-and-branch workflow to make contributions to this project's source code, tests, and documentation.

As a general rule, each contribution — identified by a unique pull request — should be restricted in its scope. Try not to mix multiple orthogonal changes together. And most changes should have a corresponding test case to go along with it.

1. ## Fork

   From this project's homepage on GitHub, click "Fork". 
   
   The repository will be cloned in your own GitHub account. The fork is your "origin" repository, while the main project repository is referred to as the "upstream" repository.

2. ## Clone

   Download a copy of your origin repository.

   ```
   git clone https://github.com/[your-github-username]/[project].git
   ```

   The copy of the project that now exists on your computer is your "local" repository. This is where you'll do your work. 

   When you ran the ``git clone`` command, Git would have automatically added a remote named "origin". You should add another remote that references the "upstream" repository. You'll need this later. From the root directory of your newly cloned local repository, run the following command in your terminal.

   ```
   git remote add upstream https://github.com/[upstream-user]/[project].git
   ```

3. ## Checkout

   In your local repository, checkout the ``test`` branch.

   ```
   git checkout test
   ```

   If you want to make changes to a legacy release of the software, checkout the relevant versioned ``test`` branch. Example:

   ```
   git checkout test/v1
   ```

4. ## Branch

   Never make commits directly into the test or production branches. Instead, branch off from the mainline to create a temporary development branch, where you will make your changes. Please use the following naming convention:

   ```
   dev/[issue]-[description]
   ```

   ``[issue]`` is the number of the issue that you opened in our issue tracker. ``[description]`` is a concise slug that describes the feature or bug. Example:

   ```
   dev/21-tweak-line-height-of-headings
   ```

   This naming convention helps the project maintainers to manage external contributions, because each pull request will explicitly reference an open issue.

   Use the following Git commands to create and switch to your development branch.

   ```
   git branch dev/[issue]-[description] test
   git checkout dev/[issue]-[description] test
   ```

   Or more succinctly:

   ```
   git checkout -b dev/[issue]-[description] test
   ```

5. ## Commit

   Do your work.

   If you will make substantive changes over a long period, you should make regular commits, organizing your changes in logical iterations.

   ```
   git add [file1] [file2] [file3]
   git commit -m "Change line-height value of document body"
   ```

   Using the ``-a`` flag on the ``commit`` command will give you the opportunity to extend the commit message — which should be short — with a more detailed description.

   ```
   git commit -a
   ```

   Be sure to add or update the relevant test cases, which are in the ``./tests/`` directory, and documentation, which is in the ``./docs/`` directory. Documentation is formatted using GitHub-Flavoured Markdown.

6. ## Synchronize

   Long-lived development branches should be kept synchronized with the project mainline, so that you're always working with the latest iteration of the source code. 
   
   Use the following commands to fetch the latest changes.

   ```
   git checkout test
   git pull upstream test
   ```

   Now return to your development branch and rebase it on the ``test`` branch. This will replay your changes after everyone else's changes. In effect, your changes stay at the tip of your development branch.

   ```
   git checkout dev/[issue]-[description]
   git rebase test
   ```

   This is a shortcut to pull and rebase from the upstream repository directly:

   ```
   git pull --rebase upstream test
   ```

   Remember, if you're introducing changes to a legacy release, be sure to rebase off the appropriate versioned ``test`` branch.

   ```
   git pull --rebase upstream test/v1
   ```

   If you get conflicts during the rebasing process, resolve them, and then continue the rebase.

   ```
   git add [file1] [file2]
   git rebase --continue
   ```

   Power users might like to try interactive rebasing. Add the ``i`` or ``--interactive`` flag to the ``git rebase`` command. You will jump into an editing buffer that will give you the opportunity to clean up your commit history. You can edit and remove individual commits, split them up, reorder them, and even squash some together. For each commit that you made, you can choose to **pick**, **squash*, **edit**, or **drop**.

   - **Pick**: This is the default behaviour when you don't do interactive rebasing. It attempts to merge the commit, but you'll be asked to manually resolve any conflicts that Git can't handle itself.
   - **Squash**: A squashed commit will have its changes folded into the contents of the preceding commit.
   - **Edit**: The rebasing process will stop and return you to the shell, with the local filesystem tree reflecting the project's state at the selected commit. The original commit's changes will be staged, ready for inclusion when you run ``git commit``. You can make changes to the original commit, before committing it again. And you can introduce additional commits. Run ``git rebase --continue`` when you're done. You'll be returned to the rebasing process where you left off.
   - **Drop**: This option removes a commit. Use this cautiously. It will cause merge conflicts if any later commits build on the dropped changes.

7. ## Push

   The process of rebasing gives you the opportunity to sort out conflicts between your work and other people's changes that were recently introduced to the project mainline. The end result is that your final PR will have a nice clean diff, so your work can be integrated faster.

   After rebasing, push your local changes to your remote origin repository. Because rebasing changes history, you should use ``-f`` or ``--force`` to force-push the new local history into the remote.

   ```
   git push -f origin dev/[issue]-[description]
   ```

   If you're working on a long-running project, you should repeat steps six and seven often. Doing so will keep your work synchronised with the upstream project, which means that your work won't diverge too far from the mainline, reducing the chances of stumbling on major merge conflicts later. And you'll have a backup of your work (on Github) in case your local repository becomes lost or corrupted.

8. ## Pull Request

   When your work is done, and with everything pushed to your origin repository, it is time to have your changes merged into the project mainline.

   Go to the Pull Requests section of the upstream repository: https://github.com/[upstream-user]/[project]/pulls.
   
   Click the "New pull request" button. Click "Compare across forks".
   
   Choose the following for merge:

   - Base repository: ``[upstream-user]/[project]``
   - Base branch: ``test``
   - Head repository: ``[your-github-username]/[project]``
   - Head branch: ``dev/[issue]-[description]``

   Pull requests should be made to the ``test`` branch of the upstream repository. Remember to choose the appropriate versioned ``test`` branch, such as ``test/v1``, if you're making changes to a legacy version of the software.

   Click the "Create pull request" button.

   Provide a concise but meaningful title for your PR. Leave a message, which must at the very least contain a link to the original issue in the project's issue tracker. Be sure to leave checked the option to "allow edits from maintainers".

   **By opening a pull request, you agree to this project's [Contributor License Agreement](cla.md).**

9. ## Tidy Up

   With everything pushed to the remotes, you can safely delete the development branch from your local repository.

   ```
   git branch -d dev/[issue]-[description]
   ```

   The maintainers of the upstream repository will review your work. If the changes are accepted, they will be merged into the upstream repository. When this is done, and when your PR has been closed by the project maintainers, it will be safe to delete the development branch from your remote origin repository. You can do this via the GitHub UI or from your local machine with the following command:

   ```
   git push origin :dev/[issue]-[description]
   ```
