**Under Construction (22 Oct 09) - working document**

## The basics ##
### Why use Version Control? ###
Even if you have only worked alone on your own scripts or website, you will know how important it is to save regularly. Version control allows you back out when things go wrong, try different options and abandon those that don't work out, not to mention just being able to go back and see what you changed.

All this becomes even more important when several developers cooperate on the same complex project.

### Centralised v Distributed ###
Older version control systems, like Subversion, use a centralised repository holding a master revision.  If you want to work on any version you need to download this from here, make your changes and push them back.  This is a classic server/client model. This means a lot of communication and someone needing to resolve the inevitable merge conflicts when there are a number of developers working on the same files.

The new generation of version control systems, of which Git is one, are known as distributed systems.  This means that When you clone the master you get a full copy of the complete history.  This requires a big investment on the repository initialisation, but after that you are working locally on your own repository speeding up most operations.

In effect this means that each cloned repository is equal and independant.  This is more a peer-to-peer model.  However, in practical terms a centralised "release version" of the project is maintained.  This also gives a reference for syncing the other repositories to.

### Introduction to Git ###
**Git** is a fast, distributed version control system, originally written for use with large repositories such as the Linux Kernel source.  As you can imagine, being designed for such large, complex applications Git is extremely powerful and complex.  However it is quite feasible to use only Git for the daily activities needed to download, build, modify, commit changes and sync the android-on-freerunner project source code.

Here are some links to more detailed information on Git:

> [The Git User's Manual](http://www.kernel.org/pub/software/scm/git/docs/user-manual.html) at kernel.org

> [A short git tutorial](http://www.kernel.org/pub/software/scm/git/docs/gittutorial.html) introduction, also from kernel.org

> [The Git website's](http://git-scm.com/documentation) own documentation page

> ["Git Magic"](http://www-cs-students.stanford.edu/~blynn/gitmagic/) a full featured web based guide

> [The Asimilatorul blog site](http://asimilatorul.com/index.php/2009/08/28/git-resources/) with lots of links to Git stuff

### Introduction to Repo ###
**Repo** is a tool that has been built on top of Git.  Specifically  it is designed to make working with Android development easier. The repo tool uses an XML-based manifest file describing where the upstream repositories are, and how to merge them into a single working checkout. repo will recurse across all the git subtrees and handle uploads, pulls, and other needed items.

Here is a quote from the android source website:
> "In most situations, you can use Git instead of Repo, or mix Repo and Git commands to form complex commands. Using Repo for basic across-network operations will make your work much simpler, however."

This is the [Android source page on using Repo and Git](http://source.android.com/download/using-repo)

## A working repository ##
### Installing Git & Repo ###

Installing Git:

The latest version of Git should be available for installation using your distribution's package manager. In Ubuntu you want the `git-core` package along with the required dependencies.

Installing Repo:
> Make sure you have a /bin directory in your home directory and that it's in your $PATH. Then run:
```
 $ curl http://android.git.kernel.org/repo >~/bin/repo
 $ chmod a+x ~/bin/repo
```


### Making your own repository ###

Create an empty directory to hold your working files.
```
   $ mkdir aof_source
   $ cd aof_source
```
Initialise your repository, linking it to the cupcake branch where current development is focused.
```
   $ repo init -u git://gitorious.org/android-on-freerunner/freerunner_platform_manifest.git -b cupcake
```

You will be asked for your name & email during the initialisation.  These are used for stamping your commits later.

Now each time you issue a Git or Repo command in this directory it will automatically be referring  to the Cupcake branch.

Synchronise the repository. _This step will take a very long time for the first sync._
```
   $ repo sync
```

## Working with the source ##
### Project layout & branches ###

**Current development is focused on the Cupcake branch**

The Android source files are divided among a number of different repositories. A **manifest file** contains a mapping of where the files from these repositories will be placed within your working directory when you synchronise your files.

The project layout is based on Android main project [Link to Android Project page](http://source.android.com/projects)

The projects are grouped in three main categories **with the addition of android-on-freerunner specific projects**:
  * Core projects:  These projects make up the foundation of the Android platform.
  * External projects:  The Android Open Source Project makes use of many other open source projects.
  * Packages:  These projects are standard Android applications and services.

### Modifying ###

Now you have your local repository where you can work on the code offline.  You can freely modify the source or develop and add your own code.

Begin by picking an issue to work on and find the relevant project/sub-directory and mark your topic branch
```
  $ repo start <branchname> <project>
```

This gives a pointer to your work within your local repository.  You can see all your topics branches using
```
  $ git branch  
```
If working on several branches the current one will be marked with a "`*`", you can move to another with
```
  $ git checkout <branchname>
```

Edit files within the </working/directory/project> using your favourite editor and changes will be recognised and tracked by Git.  Adding, deleting and renaming files should be done using
```
  $ git add <file>
  $ git rm <file>
  $ git mv <file>
```

You can check what files have been altered using
```
  $ git status
```

### Pushing & Committing changes ###

When you are happy with your changes, you should commit them to your local repository. Run
```
  $ git commit -a
```
You can also add `-m "<description of your change>"` to this command, if you don't then your default editor will open and you will be able to add a description to the commit record.

**Commit rights**
Only approved developers are allowed to push & commit to the release version of the project.  Until you reach that elevated status you need to create patches and attach them to the issue you have been working on.  The devs will review the patch and commit on your behalf.

To create the patch file
```
  $ git format-patch -<number of commits> -o <path/to/where/you/want/file/to/be/saved>
```

For example:
```
  $ git format-patch -1 -o /home/my/Desktop
```
Will create a patch file of my last commit in my Desktop directory.

The above is only a small part of what you can do with Git. Check out the links above for more details, or see this [crash course from the Git site](http://git-scm.com/course/svn.html) for a good overview of the most useful commands.

### Team Collaboration ###

You can also sync your repository with other developers work and exchange pathes to collaborate on fixes/features.  Then, when you are finished, send the patch for inclusion in the release version of the project.


---

_Send your comments to [the mailing list](mailto:android-on-freerunner@googlegroups.com)_