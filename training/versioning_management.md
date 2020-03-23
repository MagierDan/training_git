# Versioning number based on the SCM

## Why generate a version number based on the SCM
The SCM allows you to save and keep track of the code history.

Based on this, it seems pretty natural to generate your version number based on it.

This article will show you how to calculate your version number based on your SCM (here it will be GIT).

## Generate your version number with a git command: git describe

**'Git describe'**

[git describe](https://git-scm.com/docs/git-describe) allows you to find the most recent tag reachable from a commit.

1. If the tag points the commit then the command will return you the tag name.
2. Else it will return a value like this : <tag_name>-<number_of_additional_commits_since_the_tag>-g<commit_hash>

Here is the command we should use: **git describe --tags --always --dirty=-dirty**

For example if:
- the most recent tag relatively to your git history is 2.1
 - you have 7 commits since this tags
- your last commit has the following hash: 2414721b194453f058079d897d13c4e377f92dc6 
 
Then the result of the command will be 2.1-7-g2414721. Here is the explanations of the results: 
- 2.1 refers to the last tag 
- 7 represents the number of commits between the last commit and the last tag of your git history 
- g refers to GIT (See N.B. below)
- 2414721 refers to the hash of the last commit

**--tags** option explanation: 
This option allows to match any tags, annotated or not.

**--always** option explanation: 
This option allows to show uniquely abbreviated commit object as fallback.

**--dirty=-dirty** option explanation: 
If you have some local modifications not committed locally the result of the command will be 2.1-7-g2414721-dirty


**N.B.**: the 'g' before the hash in your command result stands for git. It's just in case your team uses different SCM.

## How to use the result of the command in our project

### Integration in a Java project with Maven
We will use the following command just after checkouting the code source:
**mvn versions:set -DnewVersion=$(git describe --tags --always --dirty=-dirty) -DprocessAllModules**

This command will change the version number in the pom parent and all its children.
