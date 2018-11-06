# Help Tools

This is a set of boilerplate scripts describing the [normalized script pattern
that GitHub uses in its projects](http://githubengineering.com/scripts-to-rule-them-all/). 

While these patterns can work for projects based on any framework or language, these
particular examples are for a Python application.

## The Scripts

Each of these scripts is responsible for a unit of work. This way they can be
called from other scripts.

The following is a list of scripts and their primary responsibilities.

### script/bootstrap

[`script/bootstrap`][bootstrap] is used solely for fulfilling dependencies of the project.

This can mean Python modules, Git submodules, etc.

The goal is to make sure all required dependencies are installed.

### script/setup

[`script/setup`][setup] is used to set up a project in an initial state.
This is typically run after an initial clone, or, to reset the project back to
its initial state.

This is also useful for ensuring that your bootstrapping actually works well.

### script/update

[`script/update`][update] is used to update the project after a fresh pull.

If you have not worked on the project for a while, running [`script/update`][update] after
a pull will ensure that everything inside the project is up to date and ready to work.

Typically, [`script/bootstrap`][bootstrap] is run inside this script. This is also a good
opportunity to run database migrations or any other things required to get the
state of the app into shape for the current version that is checked out.

### script/test

[`script/test`][test] is used to run the test suite of the application.

A good pattern to support is having an optional argument that is a file path.
This allows you to support running single tests.

Linting (i.e. rubocop, jshint, pmd, etc.) can also be considered a form of testing. These tend to run faster than tests, so put them towards the beginning of a [`script/test`][test] so it fails faster if there's a linting problem.

[`script/test`][test] should be called from [`script/cibuild`][cibuild], so it should handle
setting up the application appropriately based on the environment. For example,
if called in a development environment, it should probably call [`script/update`][update]
to always ensure that the application is up to date. If called from
[`script/cibuild`][cibuild], it should probably reset the application to a clean state.


### script/cibuild

[`script/cibuild`][cibuild] is used for your continuous integration server.
This script is typically only called from your CI server.

You should set up any specific things for your environment here before your tests
are run. Your test are run simply by calling [`script/test`][test].

[bootstrap]: script/bootstrap
[setup]: script/setup
[update]: script/update
[test]: script/test
[cibuild]: script/cibuild
