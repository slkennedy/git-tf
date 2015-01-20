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
but it will be useful in working with the local repository created by the tools.  Information about the latest 
Git downloads can be found on: [GitHub](http://git-scm.com/downloads)


###Installing Git-TF  
1. Extract the contents of Git-TF-Release-Preview.zip to a folder on your local machine, i.e. C:\git-tf on Windows, 
    or /user/git-tf on Mac/Linux.
2. Add the path where you extracted Git-TF (i.e. C:\git-tf) to your PATH environment variable.
3. Add the path where java.exe is installed (i.e. C:\Program Files (x86)\Java\jre7\bin) to your PATH environment variable.

##Example Usage

###Individual Developer with a New Repo  
A typical workflow for an individual developer using the Git-TF tools are as follows.  

1. git tf clone http://myserver:8080/tfs $/TeamProjectA/Main  
_Changes are cloned down from TFS to git_

2. Make changes to the file in the Git repo

3. git commit -a -m "commit one"
_commit changes locally_

4. Make more changes

5. git commit -a -m "commit two"

6. git tf pull --rebase
_Fetch the latest changes from TFS and rebase master on top of the fetched changes_

7. git tf checkin
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
