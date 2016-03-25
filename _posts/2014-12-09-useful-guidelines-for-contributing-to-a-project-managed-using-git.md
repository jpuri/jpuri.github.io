---
layout: post
title:  "Useful Guidelines for Contributing to a Project Managed Using GIT."
date:   2014-12-09 00:00
categories: General
permalink: archivers/useful-guidelines-for-contributing-to-a-project-managed-using-git
---
GIT is a popular and powerful open source version control system. In many respects it is ideal VCS if you are working in project with medium to large team size. These are a couple of useful guidelines for you if you are contributing in a project using GIT as VCS:

1. Create a branch for each logical change you are doing:

    For each logical change of yours like fixing a bug or creating a new feature avoid using master branch and rather create a new branch. This will save you from reverting your changes and rebasing your master in case your changes do not make to master branch of the main project repo.

2. Base the code you are contributing against suitable branch:

    Base your change against suitable branch, for example a new feature should ideally be rebased against master branch. A bug fix generally on maintenance branch, if the bug is reported from code of some branch not yet merge into master, base your change against that branch.

3. Do not commit white spaces:

    Most annoying change in a file on git repo are blank space, they mostly creep in because of the IDE codestyle settings. You can check you files for blank spaces introduced prior to commit using the command:

    `git diff –check`

4. Make separate commits for logically separate changes:

    For each logically separate change create a new commit. Committing together 5 small bug fixes in a commit or creating 5 different commits for a small new feature are not good idea. Logical and atomic commits help others to understand your change. They are also useful if you want to get rid of some specific code change at a later point.

    You might do frequent small commits in your branch that’s actually useful to keep your code changes safe, in that case it would be good idea to squash your commits and clean commit history before pushing your code to main repo so that everyone else gets a clean copy. But yes it is not recommended to change commit history after it is shared with others.

5. Write useful and detailed commit message:

    Your commits should have a nice detailed message that explains the change. As a rule the first line should be a concise message not more than 50 letters, that should be followed by a blank line and a detailed message. The original format specified by Tim Pope is:

        Short (50 chars or less) summary of changes_

        More detailed explanatory text, if necessary. Wrap it to about 72 characters or so.
        In some contexts, the first line is treated as the subject of an email and the rest of the text as the body.
        The blank line separating the summary from the body is critical (unless you omit the body entirely);
        tools like rebase can get confused if you run the two together.

        Further paragraphs come after blank lines.

        * Bullet points are okay, too

        * Typically a hyphen or asterisk is used for the bullet,preceded by a single space,
          with blank lines in between, but conventions vary here
          
    Also take care to describe your changes in an imperative mode.

6. Contributing to a public project – Interactive rebasing and squashing to create clean PR:

    If you are contributing your code to a public repo you may not have required permissions to directly update but will rather be submitting your changes to maintainers. A preferred way to contribute to such projects is to fork the code and then create a branch for doing changes. You can then create a pull request and submit it to maintainers.

    `git request-pull origin/master myfork`

    Interactively rebase and squashing your commits prior to creating PR can be very helpful to create clean commits and keep project history clean.  It also helps to get rid for merge commits.
