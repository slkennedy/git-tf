#Git-TF: Getting Started Guide

##Git-TF V2.0.3

###About the tools  
Git-TF is a set of command line tools that facilitate the use of a local Git repository with TFS.  These tools make it easy to clone sources from TFS, fetch updates from TFS, and to update TFS with changes committed locally in Git.    

###Release notes  
See the Git-TF Release Notes for details on new features and bug fixes.

##Installation

###Supported Platforms  
Git-TF is supported on the following platforms:  
* Windows 8.1, Windows 8, Windows 7 (x86, x64); Windows Server 2012 R2
* Linux with GLIBC 2.3 to 2.18 (x86, x64, PowerPC)
* Mac OS X 10.5 to 10.9 (PowerPC, Intel)
* Solaris 8, 9, 10 (SPARC, x86, x64)
* AIX 5.2 to 7.1 (POWER)
* HP-UX 11i v1 to v3 (PA-RISC, Itanium)

###Supported Versions of TFS  
Git-TF is supported for use with the following versions of TFS:  
* Team Foundation Server 2013
* Team Foundation Server 2012
* Team Foundation Server 2010
* Visual Studio Online

###Prerequisites  
Any of the following versions of the Java runtime (latest versions available from [Java](http://www.java.com)):  
* Oracle Java™ 1.5 to 7, or IBM Java™ 1.5 to 7 on Windows
* Apple Java™ 1.5 to 7 on Mac OS X
* Sun Java™ 1.5 to 7 on Linux or Solaris
* IBM Java™ 1.5 to 7 on Linux or AIX
* HP Java™ 1.5 to 7 on HP-UX

It is recommended that the latest version of Git is installed as well.  It is not strictly needed to use Git-TF, 
but it will be useful in working with the local repository created by the tools.  Information about the latest Git downloads can be found on: [GitHub](http://git-scm.com/downloads)


###Installing Git-TF  
1. Extract the contents of Git-TF-Release-Preview.zip to a folder on your local machine, i.e. C:\git-tf on Windows, or /user/git-tf on Mac/Linux.
2. Add the path where you extracted Git-TF (i.e. C:\git-tf) to your PATH environment variable.
3. Add the path where java.exe is installed (i.e. C:\Program Files (x86)\Java\jre7\bin) to your PATH environment variable.

##Example Usage

###Individual Developer with a New Repo  
A typical workflow for an individual developer using the Git-TF tools are as follows.  

1.    git tf clone http://myserver:8080/tfs $/TeamProjectA/Main  
_Changes are cloned down from TFS to git_

2. Make changes to the file in the Git repo

3.    git commit -a -m "commit one"  
_commit changes locally_

4. Make more changes

5.    git commit -a -m "commit two"

6.    git tf pull --rebase  
_Fetch the latest changes from TFS and rebase master on top of the fetched changes_

7.    git tf checkin  
_Check in the changes from "commit one" and "commit two" as a single TFS changeset_

###Development Team with an Existing Repo  
For a team working with an existing Git repo, a developer sharing changes to TFS using Git-TF would use the following workflow.

1. git tf configure http://myserver:8080/tfs $/TeamProjectA/Main 
_Configure the existing repo's relationship with TFS_

2. git tf pull 
_Fetch the latest changes from TFS and merge those changes with the local changes.  Note, merging is important when working in a team configuration. See "Rebase vs. Merge" below._

3. git commit -a -m "merge commit"

4. git tf checkin
_Check in the merge commit as a single TFS changeset_

5. git push 
_Push the merge commit to the origin_

##Commands  
Below is the list of commands that are currently supported by Git-TF.  These commands can be run from a command window by typing >git tf <command name>

###Help
>git tf help 
Displays the list of available Git-TF commands.


>git tf help <command>

Displays the detailed syntax for a given command, including all optional flags.


###Clone
<div class="code">git tf clone http://myserver:8080/tfs/collectionName $/TeamProjectA/Main [--deep] </div>
Initializes a new git repo from an existing path in a TFS server.  When cloning from TFS, by default a shallow clone is performed, 
i.e. only the latest changeset is downloaded and used to create the first Git commit.  The optional <span class="code">--deep</span> flag may be used to clone 
each TFS changeset for the specified path into the new Git repo.  Note, when using the <span class="code">--deep</span> option, all future <span class="code">git tf fetch</span>, 
<span class="code">git tf pull</span>, and <span class="code">git tf checkin</span> operations will default to <span class="code">--deep</span>.


###Configure 
<div class="code">git tf configure http://myserver:8080/tfs/collectionName $/TeamProjectA/Main [--deep] </div>

Configures an existing git repo to be able to communicate with a TFS server.  Like the clone command, the <span class="code">--deep</span> option may be used 
to set the default for future fetch and checkin operations.


###Checkin 
<div class="code">git tf checkin [--deep]</div>

Checks in the changes made in the Git repo into TFS.  By default, the checkin command will create a single TFS changeset for the 
aggregate of all changes made on the current branch in Git since the last checkin to TFS.  When used with the <span class="code">--deep</span> option, a TFS 
changeset will be created for each Git commit on the current branch since the last checkin to TFS.  


###Fetch 
<div class="code">git tf fetch [--deep]</div>

Fetches changes made in TFS as a new commit in Git, and references the new commit as <span class="code">FETCH_HEAD</span>.  By default, a single commit 
will be created in the Git repo with the aggregate changes since the last fetch.  When used with the <span class="code">--deep</span> option, a Git commit 
will be created for each TFS changeset that was created since the last fetch.  


###Pull 
<div class="code">git tf pull [--deep] [--rebase]</div>

Fetches changes made in TFS as a new commit in Git, and merges the commit with the latest commit in the current branch.  By default, the fetch performed by the pull 
command is shallow, but the <span class="code">--deep</span> option may be used to create a Git commit for each TFS changeset created since the last fetch.  Also, merge 
is used by default when pulling, but the <span class="code">--rebase</span> option may be used to perform a rebase instead.


###Shelve 
<div class="code">git tf shelve myshelveset</div>

Shelve the changes made in the Git repo into TFS.  Shelve operates similarly to the <span class="code">git tf checkin --shallow</span> option in that all aggregate 
changes since the last checkin will be created as a single shelveset.  


###Unshelve 
<div class="code">git tf unshelve shelvesetName [--user|-u=shelvesetOwner]</div>

Unshelve the changes from the TFS shelveset into a stash in the git repository. To apply the shelveset content in the repository
execute <span class="code">git stash apply</span>. When the stash is applied in the repository, the changes downloaded by the unshelve command will be merged with
the current <span class="code">HEAD</span>.

###Shelvesets 
<div class="code">git tf shelvesets [shelvesetName] [--user|-u=shelvesetOwner] [--details] [--delete]</div>

Lists the shelvesets available on the server that match the name and owner specified. If the <span class="code">--details</span> flag is specified, more shelveset details
will be listed. This command can also be used to delete shelvesets from the server.

##Working with Teams 

The Git-TF tool is most easily used by a single developer or multiple developers working independently with their own isolated 
Git repos.  That is, each developer uses Git-TF to clone a local repo where they can then use Git to manage their local development that will eventually be 
checked in to TFS.  In this "hub and spoke" configuration, all code is shared through TFS at the "hub" and each developer using Git becomes a "spoke".  
Developers looking to collaborate using Git's distributed sharing capabilities will want to work in a specific configuration described below.



Most often, developers collaborating with Git have cloned from a common repo.  When it comes time to share divergent changes, conflict resolution
is easy because each repository shares the same common base version.  Many times, conflicts are automatically resolved.  One of the keys to this 
merging of histories is that each commit is assigned a unique identifier that is generated by the contents of the commit.  When working with Git-TF, 
two repositories cloned from the same TFS path will not have the same commit IDs unless the clones were done at the same point in TFS history, 
and with the same depth.  In the event that two Git repos that were independently cloned using Git-TF share changes directly, the result will be a 
baseless merge of the repositories and a large number of conflicts.  For this reason, it is not recommended that teams using Git-TF ever 
share changes directly through Git (i.e. using <span class="code">git push</span> and <span class="code">git pull</span>).

Instead, it is recommended that a team working with Git-TF and collaborating with Git do so by designating a single repo as the 
point of contact with TFS.  This configuration may look as follows for a team of three developers:

<pre class="code">
[TFS]      [Shared Git repo]
|         ^ (2)  |       \
|        /       |        \
|       /        |         \
V (1)  /         V (3)      V (4)
[Alice's Repo]   [Bob's Repo]   [Charlie's Repo]
</pre>

In the configuration above the actions would be as follows:

1. Using the <span class="code">git tf clone</span> command, Alice clones a path from TFS into a local Git repo.  
2. Next, Alice uses <span class="code">git push</span> to push the commit created in her local Git repo into the team's shared Git repo.  
3. Bob can then use <span class="code">git clone</span> to clone down the changes that Alice pushed. 
4. Charlie can also use <span class="code">git clone</span> to clone down the changes that Alice pushed. 

Both Bob and Charlie only ever interact with the team's shared Git repo using <span class="code">git push</span> and <span class="code">git pull</span>.  
They can also interact directly with one another's repos (or with Alice's) , but should never use Git-TF commands to interact with TFS.  

When working with the team, Alice will typically develop locally and use git push and git pull to share changes with the team.  
When the team decides they have changes to share with TFS, Alice will use a <span class="code">git tf checkin</span> to share those changes 
(typically a <span class="code">git tf checkin --shallow</span> will be used).  Likewise, if there are changes that the team needs from TFS, Alice will perform 
a <span class="code">git tf pull</span>, using the <span class="code">--merge</span> or <span class="code">--rebase</span> options 
as appropriate, and then use <span class="code">git push</span> to share the changes with the team.

###Rebase vs. Merge 

Once changes have been fetched from TFS using <span class="code">git tf pull</span> (or <span class="code">git tf fetch</span>), those changes must either be 
merged with the <span class="code">HEAD</span>
or have any changes since the last fetch rebased on top of <span class="code">FETCH_HEAD</span>.  Git-TF allows developers to work in either manner, 
though if the repo that is sharing changes with TFS has shared any commits with other Git users, then this rebase may result in significant conflicts 
(see <a href="http://git-scm.com/book/en/Git-Branching-Rebasing#The-Perils-of-Rebasing">The Perils of Rebasing</a>).  For this reason, 
it is recommended that any team working in the aforementioned team configuration use <span class="code">git tf pull</span> with the default <span class="code">--merge</span> 
option (or use <span class="code">git merge FETCH_HEAD</span> to incorporate changes made in TFS after fetching manually).

###Recommended Git Settings 

When using the Git-TF tools, there are a few recommended settings that should make it easier to work with other developers that are using TFS.  


####Line Endings 
<div class="code">core.autocrlf = false</div>

Git has a feature to allow line endings to be normalized for a repository, and it provides options for how those line endings should be set when files 
are checked out.  TFS does not have any feature to normalize line endings - it stores exactly what is checked in by the user.  When using Git-TF, choosing
to normalize line endings to Unix-style line endings (LF) will likely result in TFS users (especially those using VS) changing the line endings back to 
Windows-style line endings (CRLF).  As a result, it is recommended to set the <span class="code">core.autocrlf</span> option to 
<span class="code">false</span>, which will keep line endings unchanged in the Git repo.


####Ignore case 
<div class="code">core.ignorecase = true</div>

TFS does not allow multiple files that differ only in case to exist in the same folder at the same time.  Git users working on non-Windows machines 
could commit files to their repo that differ only in case, and attempting to check in those changes to TFS will result in an error.  To avoid these types 
of errors, the <span class="code">core.ignorecase</span> option should be set to <span class="code">true</span>.

