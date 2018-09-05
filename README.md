# Workflow in Git
This project uses feature branches, release branches, and release tags for development and deployment.

The `master` branch shall contain the latest, fully reviewed and developer tested functionality.

**Feature** branches are temporary branches for adding one specific feature or correction. Shall be deleted when merged to `master`.

**Release** branches are temporary branches for fixing the last things before deploying a release. This could be updating changelogs, setting the version numbers, etc. Shall be deleted when merged to `master`.

## Adding functionality, correcting bugs
When a new feature is to be added, or a bug is to be fixed, a new *feature branch* shall be created from either `master`, or the release branch the fix is going into.

Feature branch names shall start with the JIRA id, and then a short but meaningful name in lowercase letter and words joined together with hyphen, e.g.:

`JIRA-55-add-new-io-signals`

Including the JIRA id in the name makes JIRA detect the branch, and the branch will be listed on the issues page. It also makes it easier for other developers to know what task is being worked on.

Start developing the feature or fixing the bug. Make commits as you please, but please keep them meaningful.

When the work is coming to an end and has been tested by a developer, make a pull request to the `master` branch github. This creates a review, that should be assigned to someone else than the code author. This is both to raise code awareness, and also quality. Any comment shall be addressed, either fixed or answered why no change is required.

When the reviewer(s) has approved the work, the pull request shall be merged to `master`. A merge commit shall always be created, i.e. avoid fast-forward merges. This makes the development history easy to follow, as the `master` branch will contain only merge commits, and it is easy to look at the commit history of one particular feature branch.

After merging, the code task is complete and the feature branch shall be deleted. Deleting the branches after merging allows for a quick overview on what work is currently active by checking which branches are present. Lingering finished branches pollutes the overview.

## Releasing
When a release is to be made, a new release branch shall be created from `master`, or if the release is a fix of a previous release, from the tagged release (more information about tagging follows).

Make sure that the branch which the release branch is created from contains all the functionality that shall go into the release, and **only** the functionality needed for the release, meaning that any feature that has been implemented but should not be included in the release shall be left out.

Release branches shall be named starting with the prefix **r-** and then the version number. The version number is formatted as three numbers separated with dots, *major*.*minor*.*patch*. An example would be:

`r-2.1.1`

* *Major* version should be increased when the code is no longer backwards-compatible, e.g. when existing API has changed, or if a substantial rebuild of the code has been done.
* *Minor* version should be increased when the code supports new features, but API stays backwards-compatible.
* *Patch* version should be increased when no new features has been added, but improvements have been made to previous functionality, e.g. bug fixes.

(See documentation on [Semantic Versioning](https://semver.org/) for more details)

The build system has been configured to use the version number in the branch name for generating correct versioned binary artifacts, e.g. assemblies, nuget packages, so be sure to get it right.

When all changes necessary to do a release has been done, everything builds on the build system and all tests <span style="color: green">PASS</span>, the release is ready. The last commit on the release branch shall be tagged in Git. The tag shall be named with the prefix **release-** (so not to mix up the branch name and the tag name) followed by the version number of the release branch. An example would be:

`release-2.1.1`

The tag will be detected by the build system.
**It is the tagged release build's artifacts that shall be deployed**.

After tagging, the release branch shall be merged to master. A merge commit shall always be created, i.e. avoid fast-forward merges. When the branch has been merged, it shall be deleted.
