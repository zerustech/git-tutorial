Symfony GitHub Best Practices
==============================

Fixing bugs / making minor changes in Symfony
------------------------------------------------

During the lifetime of a minor version, bugs as well as other minor changes can be commited to the minor versionand be released as patch version on regular (monthly, e.g.) basis.

This document describes the bondaries of acceptable changes.

1. Visit [symfony version checker][1] to find the oldest active version that is affected by this bug. At the time of documentation, the version should be ``2.7``, which will be referred to as the base branch hereinafter.

1. Create a topic branch based on the base branch.

   ```bash
   $ checkout -b topic-<topic name> <base branch>
   ```

1. Fixing bug on the topic branch.

1. Rebase the topic branch on top of the latest base branch.
   ```bash
   $ # Updating
   $ git fetch upstream
   $ git checkout <base branch>
   $ git pull . upstream/<base branch>
   $
   $ # Rebasing
   $ git checkout topic-<topic name>
   $ git rebase -i <base branch>
   ```

1. Push the topic branch to github. 

   ```bash
   $ git push --set-upstream origin topic-<topic name>
   ```

1. Create a Pull Request (PR) for this topic branch. 

1. The manager reviews the PR and provides feedbacks.

    1. If there is anything wrong with the topic branch, fix the issues and push
       again.

1. If everything looks fine, the manager merge the topic branch on top of the
   bash branch.

::: info-box note

Besides bug, the following changes can also be included in a patch release:

* Performance improvement
* Coding standard and refactoring 
* Tests
* Translations
* External data
* Version udpates for Composer dependencies
* Newer versions of PHP/HHVM
* Newer versions of popular OSes

Refer to [symfony maintenance policy][2] for details.

:::

::: info-box tip 

All bug fixes merged into maintenance branches are also merged into more recent branches on a regular basis (per week, 
for example). For instance, if you submit a patch for the 2.7 branch, the patch will also be applied by the core team on
the master branch.

:::

Merging bug fixings / minor changes
------------------------------------

On regular basis (weekly, e.g.) all bug fixes and minor changes merged into
maintenance branches are also merged into more recent branches.

For example:

```bash
$ git checkout 2.7
$ git merge 2.3       # merge 2.3 into 2.7
$ git push
$ 
$ git checkout 2.8 
$ git merge 2.7       # merge 2.7 into 2.8
$ git push
```

Making a patch release
----------------------

```bash

$ git fetch upstream
$ # Merge all older branches into current minor version branch
$ # git merge 2.7, for example
$ git tag v<MAJOR>.<MINOR>.<PATCH>
$ git push
$ git push --tags

```

Adding a new feature in Symfony
--------------------------------

During the lifetime of a major version, new features can be commited to the master branch and be released as minor version on regular (monthly, e.g.) basis.

This document describes the bondaries of acceptable changes.

1. Create a topic branch based on the master branch (master is always the dev
   branch for the current minor version).

   ```bash
   $ git checkout -b feature-<feature name> master
   ```

1. Developing the feature on the topic branch.

1. Rebase the topic branch on top of the latest master.

   ```bash
   $ # Updating
   $ git fetch upstream
   $ git checkout master
   $ git pull . upstream/master
   $ 
   $ # Rebasing
   $ git checkout feature-<feature name>
   $ git rebase -i master
   ```

1. Push the topic branch to github.

   ```bash
   $ git push --set-upstream origin feature-<feature name>
   ```

1. Create a Pull Request (PR) for this topic branch. 

1. The manager reviews the PR and provides feedbacks.

    1. If there is anything wrong with the topic branch, fix the issues and push
       again.

1. If everything looks fine, the manager merge the topic branch on top of master.

::: info-box tip 

The following changes must NOT be committed to the master branch:

* Backwards compatibility breaks - (e.g, remove deprecated items)
* Support for external platforms 
* Exception messages.
* Adding new Composer dependencies
* Support for newer major versions of Composer dependencies.
* Web design.

These changes must be included in a major release.

:::

Making a minor release
----------------------
```bash
$ git fetch upstream
$ # Merge all older branches into master
$ # Edit composer.json (dev-master: <MAJOR>.<MINOR>) and commit the change.
$ git checkout -b <MAJOR>.<MINOR>
$ git push --set-upstream origin <MAJOR>.<MINOR>
```

Adding backwards compatibile breaks in symfony
----------------------------------------------

Any changes that break the backwards compatibiliy must be included in a major
release.

1. Fork a major development branch, if it does not exist.

   ```bash
   $ git checkout -b <MAJOR>-dev master
   ```

1. Working on this branch

Making a major release
----------------------

```bash
$ git fetch upstream
$ # merge all older branches into master, then merge <MAJOR>-dev into master
$ git checkout master
$ git merge 2.8
$ git git merge <MAJOR>-dev
$ # Edit composer.json (dev-master: <MAJOR>.0-dev)
$ git checkout -b <MAJOR>.0
$ git push --set-upstream origin <MAJOR>.0
```

[1]: http://symfony.com/roadmap?version=2.0#checker "The symfony version checker"
[2]: http://symfony.com/doc/current/contributing/code/maintenance.html "The symfony maintenance policy" 
