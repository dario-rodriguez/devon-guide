:toc: macro
toc::[]

= Working with Git and Github

== What is a version control system?

A version control system (VCS) allows you to track the history of a collection of files. It supports creating different versions of this collection. Each version captures a snapshot of the files at a certain point in time and the VCS allows you to switch between these versions. These versions are stored in a specific place, typically called a repository.

== What are Git and Github?

Git is currently the most popular implementation of a distributed version control system.

Git originates from the Linux kernel development and was founded in 2005 by Linus Torvalds. Nowadays it is used by many popular open source projects, e.g., the Android or the Eclipse developer teams, as well as many commercial organizations.

The core of Git was originally written in the programming language C, but Git has also been re-implemented in other languages, e.g., Java, Ruby and Python.

You can use on Windows by installing, among others, https://git-for-windows.github.io/[Git for Windows].

GitHub is a web-based Git repository hosting service. It offers all of the distributed revision control and source code management (SCM) functionality of Git as well as adding its own features.

Both the Open Source as well as the IP parts of Devon are hosted on Github. The workflow is based on the default workflow as being supported by Github. 

== Git Devon/OASP4J Workflow

When you work with Git on a https://github.com/devonfw/devon[Devon] / https://github.com/oasp/oasp4j[OASP4J] project you need to take into account that you are always working with a local copy of the remote repository. 

The next image can help you to have a clear view about the way to work with Git in a https://github.com/devonfw/devon[Devon] / https://github.com/oasp/oasp4j[OASP4J] project.

image::images/devon-guide-working-with-git-diagram.PNG[,scaledwidth=80%]

=== Step 1 - Create a new Fork

To avoid problems with OASP/Devon repositories we need to create our own copy (a "fork") of the repository.

To create the repository we need to go to the GitHub repository of https://github.com/devonfw/devon[Devon] / https://github.com/oasp/oasp4j[OASP4J] and then click on the option Fork.

image::images/devon-guide-working-with-git-fork.PNG[,scaledwidth=80%]

A fork is a copy of a repository. Forking a repository allows you to freely experiment with changes without affecting the original project.

Most commonly, forks are used to either propose changes to someone else's project or to use someone else's project as a starting point for your own ideas.

=== Step 1 - Clone the repository 

Now we are going to copy to our machine the repository that we created in the last step. This is called "making a clone". To do this we need to open a console window (for example: GitBash) and then include the next command lines in the directory where we want to create the local copy of the remote repository.

Devon
[source,console]
----
git clone https://github.com/<your_git_user>/devon
----

OASP4J
[source,console]
----
git clone https://github.com/<your_git_user>/oasp4j
----

Now we have a local copy of the repository.

=== Step 3 - Define the repository URL

To avoid problems with the Git URLs repositories we are going to redefine the label used by git as a shortcut for the repository´s URL. The standard label "origin" will be replaced by our GitHub username.

To do this we need to open the console and go to the local repository and then execute the next command lines.


[source,console]
----
git remote add devon https://github.com/devonfw/devon
----

Or
 
[source,console]
----
git remote add oasp https://github.com/oasp/oasp4j
----

Now you can see the remote repositories with the command line 

[source,console]
----
git remote -v
----

If you are defining Devon URL you will see something like this

[source]
----
$ git remote -v
devon   https://github.com/devonfw/devon (fetch)
devon   https://github.com/devonfw/devon (push) 
origin  https://github.com/<your_git_user>/devon (fetch)
origin  https://github.com/<your_git_user>/devon (push)
----

If you are adding OASP4j

[source]
----
$ git remote -v 
oasp    https://github.com/oasp/oasp4j (fetch)
oasp    https://github.com/oasp/oasp4j (push)
origin  https://github.com/<your_git_user>/devon (fetch)
origin  https://github.com/<your_git_user>/devon (push)
----

Now we are going to rename the origin remote repository the with this command line

[source]
----
git remote rename origin <your_git_user>
----

=== Step 4 - Working with Topic Branches

The last steps were a introduction about how you can get the remote repositories on your local machine. Now we need to work with this repository. To do this we need to create a new topic branch. 

Topic branches are typically lightweight branches that you create locally and that have a name that is meaningful for you. They are where you might do work for a bug fix or feature (they're also called feature branches) that is expected to take some time to complete.

Another type of branch is the "remote branch" or "remote-tracking branch". This type of branch follows the development of somebody else's work and is stored in your own repository. You periodically update this branch (using git fetch) to track what is happening elsewhere. When you are ready to catch up with everybody else's changes, you would use git pull to both fetch and merge.

To create a new topic branch you need to use the next command line

[source,console]
----
git branch <new_branch_name>
----

To see the actual branch you can use the next command line

[source,console]
----
git branch 
----

To see all branches you can use the next command line. Also you can use this command to see the actual branch because it is shown with an asterisk.

[source,console]
----
git branch -a
----

To move to another branch you need to use 

[source,console]
----
git checkout <name_of_existing_branch> 
----

=== Step 5 - Commit the changes

When you are working in a branch and you want to change the branch or you just want to save your change in your local repository you need to do a commit.

To commit your changes you need to use the next command line.

[source,console]
----
git commit -m "Commit message"
----

With this line git stores the current contents of the index in a new commit along with a log message from the user describing the changes.

In several cases you will see a message like this

[source]
----
$ git commit -m "Commit message"
On branch new_branch
Changes not staged for commit:
        deleted:    README.md
        modified:   pom.xml

Untracked files:
        New Text Document.txt

no changes added to commit 
----

As you can see git tells us the changes we have in the branch and we need to add the file "New Text Document.txt". There are several way to add a new file on git.

You can add file by file with the command

[source,console]
----
git add <file_name>
----

[NOTE]
==== 
You need to keep in count that if you have some space in the name of the file you need to add the name like 
[source,console]
----
git add File\ With\ Spaces.txt
---- 
====

Another way to add the files is the next

[source,console]
----
git add .
----

This command line add all untracked files in the local repository, this is a little bit dangerous because in some cases we don't want to add some files like for example Ecplise configuration files.

In this case we need a way to exclude or ignore some files. Git have a file called .gitignore where you can put the files to ignore. The competent of the file is something like this

[source]
----
*.class
*.classpath
*.project
*.iml
.*
target/
jsclient/
eclipse-target/
**/src/generated/
**/tmp/

# Package Files #
*.jar
*.war
*.ear

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
----

As you can see there are many extensions and folders that Git will ignore if you use the command "git add .".

[NOTE]
==== 
Windows does't permit us to create a file with the name ".gitignore so to create a new .gitignore file you can use the next command line

[source,console]
----
echo "" > .gitignore
git add .gitignore
----

The we can open the file we just to create with a text editor and include whatever ignore we want.
==== 

Another way to commit without problems is commit and add the files at the same time, you can do this with the command

[source,console]
----
git commit -am "Commit message"
----

You need to keep in count the .gitignore file in this case too.

=== Step 6 - Push to remote repository

When we have the changes we want to include in the repository we need to include this changes in our remote repository. To do this we need to push our local topic branch in remote branch. 

[source,console]
----
git push <remote_repository> <topic_branch_origin>:<topic_branch_destiny>
----

As you can see the <remote_repository> can be the URL of the GitHub repository or the name that we define in the step 3.

=== Step 7 - Pull request

At this point we have the modifications in our remote repository, so we need to have now a pull request to the remote https://github.com/devonfw/devon[Devon] / https://github.com/oasp/oasp4j[OASP4J] repository. To do this we need to go to our fork repository of https://github.com/devonfw/devon[Devon] / https://github.com/oasp/oasp4j[OASP4J], open the branch we want to pull and then press the button "New pull request".

image::images/devon-guide-working-with-git-new-pull-request.PNG[,scaledwidth=80%]

First of all, GitHub will check if the branch is correct and is available to do the pull request. If all is correct you will see something like this

image::images/devon-guide-working-with-git-available-to-pull.PNG[,scaledwidth=80%]

As you can see the branch is available to do the new pull request, also you can check down in the page the changes with respect to the original repository. 

When we check that all is correct we can press the button "Create pull request" and create the new pull request. Then we can see a little form with a name of the New pull request and a little description that we need to complete. 

image::images/devon-guide-working-with-git-new-pull-request-description.PNG[,scaledwidth=80%]

When we complete the form we press the button "Create pull request" and then the pull is sent to be checked and added in the remote original repository.

=== Step 8 - Synchronize the repository 

When our Pull request is included in the original repository we need to actualize our local and remote repository with the original repository. To do this, first of all we need to check we are in the develop branch.

[source,console]
----
git checkout develop
----

Now we need to pull the original https://github.com/devonfw/devon[Devon] / https://github.com/oasp/oasp4j[OASP4J] repository to our local repository. To do this we execute the next command line

[source,console]
----
git pull devon/oasp develop:develop
----

As you can see we can use the define variables with the url of https://github.com/devonfw/devon[Devon] / https://github.com/oasp/oasp4j[OASP4J] (Step 3) or just the URL of the repository.

When you have the local repository synchronized you need to push the local develop branch to your remote develop branch

[source,console]
----
git push <your_git_user> develop:develop
----

As is commented above <your_git_user> is the variable define with the URL of your remote repository (the fork of https://github.com/devonfw/devon[Devon] / https://github.com/oasp/oasp4j[OASP4J]) (Step 3).