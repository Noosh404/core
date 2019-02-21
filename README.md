[![Build Status](https://travis-ci.org/nickrusso42518/core.svg?branch=master)](https://travis-ci.org/nickrusso42518/core)

# PM TN Config Manager core
Contains universal logic for building specific platforms and is
represented in an Ansible role. A basic playbook is consumable as well,
but individual unit-defined variables are contained in unit-specific
repositories.

## For Units
This section is for end users only.

### Getting started
These steps are for first-time users who need to grab the relevant content.
You'll need to clone two repositories, `core` and your specific unit one.
Find your unit in the list of repositories and substitute it in below
instead of the text `bde1`.

```
git clone --branch master --single-branch git@github.com:nickrusso42518/core.git
git clone --branch master --single-branch git@github.com:nickrusso42518/bde1.git
```

Navigate into your unit specific folder and run the infra-as-code playbook,
which will apply the TNIC baseline configuration to your managed nodes.

```
cd bde1/
make
```

You can safely run `make` over and over without causing an issues as the code
enforces compliances with the baseline.

### Customizing unit data
If users need to add, modify, or delete basics parts of their configuration,
such as firewall rules, interfaces, VLAN IDs, or NTP servers, begin by
modifying specific variables in the unit-specific variable repository. Once
complete, use basic `git` SCM workflows to version control the changes.

### Customizing the baseline
TNIC baselines cannot cover every conceivable use case. You may update the
templates in the `core/roles/iac/templates/` path to redefine your unit's
customized baseline. This should be less common than changing unit variables,
but is supported. Use basic `git` workflows to manage it.

### Other handy commands
The following are other shortcut commands to simplify operations defined in
the `Makefile`:

1. `make all`: Same behavior as plain `make`; executes infra-as-code
1. `make update`: Downloads and merges the newest updates from TNIC
2. `make dry`: Shows what *would* be changed if `make` were run, but
   does not make any changes

## For program offices
This information is for PMs that interact with TNIC.

### Requesting change requests of TNIC
Changes to both unit data and templates are relevant for TNIC. PMs should
clone the `core` repository along with *any* of the unit directories.

```
git clone git@github.com:nickrusso42518/core.git
git clone git@github.com:nickrusso42518/bde1.git
```

Use the Github GUI mechanics to create a new issue. Be very specific
about the problem or enhancement being suggested. Reference specific
lines of code in specific files. Provide context around why the fix
is useful to the end users. Assume that the issue is assigned `#4`
which can be referenced in commit messages later.

### Developing changes
Before making any changes, create a new feature branch with an intuitive
name, such as "newvlan57", in each repository that requires changing. If
only the templates change, there is no need to create a feature branch
in the unit data. If only unit data is being updated, the core repository
doesn't need a feature branch.

```
cd bde1/
git checkout -b newvlan57
```

Begin making your appropriate changes to the code. Use basic `git` SCM
actions to properly version control it, and be sure to push the changes
to the origin repository without locally merging back into master. Be
sure to reference issue `#4` so that the messages are automatically
rolled into the issue comments. Note that branches to `master` are
*never* allowed; always use a feature branch.

```
git add .
git commit -m "WIP #4, added the VLAN interface"
git push -u origin newvlan57
```

From there, submit a new pull request (PR) using the Github GUI mechanics.
Your PR will *not* be accepted unless all CI tests pass. Upon peer review
by senior PM and TNIC engineers, the following actions will occur:

1. `origin/newvlan57` will be merged into `origin/master`
2. `origin/newvlan57` will be deleted
3. TNIC will offer any final comments and close the issue.

You must perform the following actions to consume the update:

1. `git fetch -p`: Pull down changes after TNIC's merge
2. `git checkout master`: Switch to local `master` branch
3. `git merge origin/master`: Merge changes from TNIC into local `master`
4. `git branch -d newvlan57`: Delete the old local `newvlan57` branch
