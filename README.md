# Git Submodule
## Advantages
- Built-in to Git.
- Widely used and documented.

## Disadvantages
- Adding and updating dependencies uses a command line interface, which
requires a deeper understanding of Git and submodules.

# NCAR manage_externals
## Advantages
- Uses a configuration file to define external dependencies.
- Updating external dependencies just requires an update to the dependency
definition in the configuration file.
- Supports other SCM protocols like Subversion.

## Disadvantages
- Additional dependency to manage in Git repositories and/or build pipeline.
- If manage_externals is added to a Git repostory, as with the
ufs-community/ufs-srweather-app project, Git submodules should arguably be
used to manage that dependency. However, this means two tools will be used
to manage external dependencies.

# Recommendation
While NCAR's manage_externals provides an easy-to-use interface, I would
recommend using Git submodules for managing external dependencies.

Git submodules is built-in to Git, which is a required component of the CI/CD
pipeline. The Git submodules interface is not as intuitive as
manage_externals, but this can be overcome with documentation and use.

Git is maintained and supported by all major Linux distributions, whereas the
support and distribution for NCAR's manage_externals is not defined at this
time.  Therefore, this would likely be another dependency the platform team
would be responsible for. In other words, the platform team would need to
create an additional build agent for projects that use manage_externals.

The standardization of build tools and processes is another factor. Projects
that use manage_externals would use a different build process than every
other application that uses Git submodules.

# Reproduction of ufs-srweather-app manage_externals Configuration with Git Submodules
The following commands may be used to set up the dependencies of the
ufs-community/ufs-srweather-app project with Git submodules.

```sh
git submodule add https://github.com/NOAA-EMC/regional_workflow
cd regional_workflow
git checkout 7da0c46
cd ../
git add regional_workflow
git commit
git submodule add https://github.com/ufs-community/UFS_UTILS src/UFS_UTILS
cd src/UFS_UTILS
git checkout ea821358
cd ../../
git add src/UFS_UTILS
git commit
git submodule add https://github.com/ufs-community/ufs-weather-model src/ufs-weather-model
cd src/ufs-weather-model
git checkout 3b8bb78
cd ../../
git add src/ufs-weather-model
git commit
git submodule add https://github.com/NOAA-EMC/UPP src/UPP
cd src/UPP
git checkout a49af05
cd ../../
git add src/UPP
git commit
```

On subsequent clones of the repository, the following commands would be used
to download the dependencies.
```sh
git submodule init
git submodule update
```

