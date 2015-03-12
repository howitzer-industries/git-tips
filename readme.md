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
-   Publishing a new repo to GitHub from SourceTree is a bit of a pain. I prefer to create the repo in SourceTree then publish it using the GitHub client.
-   Beware of discarding hunks in SourceTree when you have the line endings set up to auto-convert from Windows/Mac to Unix in the repo...it'll replace all the other line endings in your file with Unix ones and mess up your diff. I prefer to stage hunks when possible then discard the rest after the commit.

##.NET
-   For your NuGet packages, I've found it easier to just ignore the local packages folder and use NuGet's package restore to replace them on build environments and other developer's machines. To ignore the local copies without missing the `repositories.config` file, add a `.gitignore` file with the following contents in `{solution_root}\packages`.
```
*
!.gitignore
!repositories.config
```
-   New versions of Visual Studio will automatically run `nuget restore` to get the new packages, but AppVeyor requires a pre-build script to call it. To configure that, add `nuget restore` to your pre-build script, either through the interface or in `appveyor.yml`:
```yml
before_build:
  - nuget restore
```
