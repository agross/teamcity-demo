# TeamCity Labs

## Keep in mind

* Start simple
* Find a way to organize your build configurations so you can parallelize work
  across multiple agents
* Try to minimize duplication (e.g. by using templates)
* Find the stuff that differs between builds and make it a parameter
* Try to organize build configurations and parameters such that it is easy to
  get an overview

## Your tasks

1. Clone the project to somewhere you have write access (e.g. internal Git
   Server)
1. Check out what the `tasks` directory provides and run `./run-me` with some of
   the tasks
1. Create a new TeamCity project, attach it to your Git server and `compile` the
   project
1. Harvest artifacts produced by the compilation
1. Check out the `early-artifacts` task and use that as an alternative
   implementation of the last step
1. Create a build configuration to run unit and integration tests (using the
   artifacts from `compile`)
1. Split the test run into `test` (unit tests) and `integration-test`
1. Split `integration-test`s by category: `slow`, `database` and `browser` and
   parallelize them
1. Run the `browser` integration test category on a build agent that has Chrome
   installed - How would you try to find a matching agent?
1. If all tests are successful, `deploy` the app
1. How could you structure your build configurations such that the deployment
   does not depend on each and every integration test category?
1. Add a Build Report tab to display the deployment log
1. Add a Project Report tab showing the last successful deployment log
1. What happens if you break unit or integration tests? (See below for the
   environment variables you need to set.)
1. Set up the build such that it's not possible to remove tests without breaking
   the build. (See below for the environment variables you need to set to reduce
   the number of tests.)
1. Generate the version number during compilation (`version` task) and reuse the
   version for deployment
1. Extend the build to be able to run feature branches
1. Change the deployment such that you can only deploy from master, triggered
   manually
1. Run tests automatically when a commit is pushed to master or any other branch
   (and deployment is still triggered manually!)
1. Export your configuration to Kotlin and save it next to the source code (i.e.
   same VCS root)
1. Apply changes to the Kotlin configuration, e.g. by adding an additional build
   step in a feature branch, push it and see what happens

## Build script

```sh
# The script supports running tasks from the tasks directory:
#
# ./run-me [task...], e.g. ./run-me compile test
#
# The compile task creates:
# * a binary under build/bin/binary (with four libraries that get loaded)
# * unit tests
# * integration tests
#
# You can control what the compile task generates by setting the following
# environment variables:
#
# UNIT_TESTS: number of unit tests, 10 by default
# UNIT_TEST_FAILURE: if nonempty, one unit test will fail
# INTEGRATION_TESTS: number of integration tests per {browser,slow,database}
#                    category, 5 by default
# INTEGRATION_TEST_FAILURE: if nonempty, one integration test per category will
#                           fail
# TEST_FILTER: category of integration tests to run {browser,slow,database}
# BUILD_NUMBER: used as input to the build metadata for the generated version
# BUILD_VCS_NUMBER: used as input to the SHA metadata for the generated version
```
