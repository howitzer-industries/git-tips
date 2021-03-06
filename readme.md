# Git / SourceTree Tips

## Git
-   Git doesn't track empty folders, so you won't see them in your unstaged items. This can cause problems if you go to discard changes, because the file will be deleted but the empty folder structure will remain. Before you go to start writing a Powershell script like I did, check out `git clean -d -n` which will show you a list that includes all the empty folders that you can't see anywhere else.
-   If you want to source control an empty folder, and **include** any files you add to it later, add a `.gitignore` file in the root of that folder containing:
```
!.gitignore
```
-   If you want to source control an empty folder, and **exclude** any files you add to it later, add a `.gitignore` file in the root of that folder containing:
```
*
!.gitignore
```

## SourceTree
-   Publishing a new repo to GitHub from SourceTree is a bit of a pain. I prefer to create and publish the repo in the GitHub client and then add it to SourceTree. This way you get the automatic .gitignore info.
-   Beware of discarding hunks in SourceTree when you have the line endings set up to auto-convert from Windows/Mac to Unix in the repo...it'll replace all the other line endings in your file with Unix ones and mess up your diff. I prefer to stage hunks when possible then discard the rest after the commit.

##.NET
-   For your NuGet packages, I've found it easier to just ignore the local packages folder and use NuGet's package restore to replace them on build environments and other developer's machines. To ignore the local copies without missing the `repositories.config` file, add a `.gitignore` file with the following contents in `{solution_root}\packages`.
```
*
!.gitignore
!repositories.config
```
-   New versions of Visual Studio will automatically run `nuget restore` to get the new packages, but AppVeyor requires a pre-build script to call it. To configure that, add `nuget restore` to your pre-build script, either through the interface or in `appveyor.yml` to something like this:
```yml
environment:
  solution_file: folder\mysolution.sln

build:
  project: $(solution_file)

before_build:
  - nuget restore %solution_file%
```

## Git Flow
Git flow may break locally if you delete one of the branches it relies upon (`master`, for instance). It'll give you a message saying it's not initialized.

## svn2git
`svn2git` is a utility to pull from an SVN repository and create a git repo from the history. The following command can be used to mirror a folder in an SVN repo to its own git repo.

```
svn2git https://my.svn.server/svn/repo_name/trunk --no-minimize-url --trunk {name of the folder} --nobranches --notags --username {mirroring username} --authors ~/svn_authors.txt
```

Create a file matching the SVN users to git emails at `~/svn_authors.txt` in the format `{svn username} = {git full name} <{git email}>`

```
first.user = First User <first.user@example.com>
second.person = Second User <second.user@example.com>
alsothisperson = Also This Person <otherguy@otherexample.com>
```
