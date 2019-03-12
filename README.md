# Software Development and Release Process

Version 1.0
	

# Introduction
This document is a brief overview of the cloud based software development and release process. It is an agile process with a focus on rapid and consistent delivery of working software.

**NOTE:** The terminology and procedures are based on using GitHub as the source code repository. Different Git vendors may use different terms but the processes are the same.

# Principles
1. Code on the master branch must be releasable at all times.
2. Work in small increments and commit often.
3. Test as you go rather than leaving testing to the end.
4. Perform reviews on work to ensure good team communication.
5. Keep track of technical debt and allocate time to review/remediate it.

# The Process
The process begins with requirements from users, product owners, stakeholder and the development team. These requirements are captured in the form of user stories. The user stories are collected in a backlog.

The backlog is periodically reviewed and stories are selected for implementation. The work on implementing the stories may be done in sprints or in a continuous delivery model.

# User Stories and Design Documents
User stories form the basis for any work done on the project. User stories capture requirements in a very simple way. They are an expression of a user’s need in the form “As a user I would like to <insert problem statement>”. 

Depending on the scope of a user story, an accompanying design document may be written. The design document explains what will be achieved, i.e. the goal of the work, possible options for implementation, options for the UI/UX and defines a set of tests to ensure that the goal was achieved.

The design documents are reviewed by the team or at least relevant members of the team prior to the story being scheduled for implementation. By planning the work ahead of time, and having it reviewed you can tap into the collective knowledge and problem solving skills of the team to provide the best solution.

A standardized design document template is available and should be used to ensure that the salient information is captured.

# Test Driven Development (TDD)
Writing tests while you develop your feature is strongly encouraged. It is much easier and faster to perform red/green development while you’re still developing your feature than it is to add tests after the feature is complete.

TDD goes hand in hand with the story development where you define your major acceptance tests, i.e. the contract the feature will fulfill, while you are working through your design choices.

**Pro Tip:** While you are building your feature you may create “scaffolding” tests that test particular internal aspects of the feature. Once development is complete you can delete these scaffolding tests leaving the core tests which ensure that the feature is fulfilling its contract with the system. By removing the scaffolding test you keep your test suite from ballooning with tests that are no necessary to ensure the proper functioning of the system.

# Git Workflow 

The Git workflow is based on the premise that changes to the source code are made on development branches that are then merged into the master branch before being, in turn, merged or cherry picked onto a release branch for deployment. 

Alternately deployments may be driven directly from master but this requires that you have measures in place to manage incomplete work, e.g. feature flags or merge lockouts while deployment is in progress.

Branch protection is key aspect of managing the release process. For example no one is allowed to commit a code change directly to a release branch. Release branches can only be changed by merges from the master branch. The master branch is also protected in that all merges can only occur from an approved pull request.

# Feature Development
Developers “clone” the Git project on their local PC. Work is performed on a branch that is created from the master branch for each task that is being worked on. These topic (or feature) branches, only exist for the duration of the work being done on the feature.

Prior completing the work, the developer runs the full test suite to ensure that the work is correct.

# Pull Requests, Code Review and Merging
When the developer is has successfully passed the local checks, he or she pushes the topic branch to the GitHub repo. Next the developer opens a Pull Request (PR) to have the code changes reviewed before merging into the master branch. With the PR open, the developer can continue to push updates and further changes to the same PR as a result of review comments. 

A PR should require that at least one other team member review and approve the changes.

When the PR has been approved, the changes can be merged into the master branch. This can be done by the final reviewer or in most cases the reviewers simply post their approval and the submitting developer performs the merge.

**Pro tip:** GitHub does not enforce the use of PRs out of the box. To require the use of PRs enabled branch protection rules under ‘Branches’ in ‘Settings’.


**Pro tip:** After the code has been merged, it is a good practise to clean up any unused topic branches both locally and on GitHub. When this occurs, depends on the subsequent work to be done.

# Continuous Integration (CI)
Developers are required to run test suites locally before committing. Those same tests will be run automatically when a topic branch is pushed to the GitHub repo and also when the topic branch is merged into the master branch.


The CI server is responsible for building the code and running the tests on branch pushes. The results of the build and test runs are pushed to GitHub and reported in the PR as a “status” field. If there are any errors then they will have to be resolved before the PR can be merged.

# Deployment
Deployment should be a simple push button action performed via the CI/CD tool. It is critical that any manual steps be automated as the risk of human error is greatest during deployment. Also automated deployment encourages more frequent deployment - an agile principle.

# Deployment Rollback
On occasion code will be released that has an error or was released before a feature was complete. Having the ability to rollback to a previous release is very important. Deploying a previous build is usually straightforward, but sometimes releases contain destructive changes to database schema(s) for example. Good planning is required to allow a rollback in such cases. A good practise is to design your database migration strategy with rollbacks in mind. For example if you plan to replace an old field with a new one, delay removal of the old field from the database to a future release when you are certain you will no longer need the old field.

**Pro Tip:** It is good practise to actually test rollback once in a while to make sure that you can do it without bringing your app down.

# Merge Failures
On occasion two developers will make changes in the same areas of the codebase at the same time. When this occurs, the developer who merges last will encounter a merge failure. In a direct commit model (i.e. developers have access to master directly) a merge failure would happen on the developer’s PC and local tools would be used to resolved the failure. With a PR based merge, the merge happens on GitHub. GitHub has a facility to resolve simple merge failure online. But, if the merge is more complex you may need to checkout the pull request locally to resolve the problems. 

This process is described here: https://help.github.com/en/articles/checking-out-pull-requests-locally 

https://help.github.com/en/articles/resolving-a-merge-conflict-using-the-command-line

# Notifications
It is a good practise is to connect the build tools and GitHub to a messaging system such as Slack. Build failures, test failures, merge failure and, of course, successful builds are good candidates for notifications.

**Pro Tip:** To keep the messages to a minimum, only fire messages when state changes. For example if a build fails, it may take a couple of attempts to get it working. There’s no point in notifying until the build succeeds again.
