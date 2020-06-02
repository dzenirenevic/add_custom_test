# Dependency tracked testing functionality for CMake

## Motivation

CMake's built-in `test` target runs all tests that have been declared as part of the project. While this behavior is suitable for the initial build or in CI, if it is used in incremental builds as part of the development cycle, it triggers all the tests in a project regardless of any change of the dependencies of test executables. While CMake provides the capabilities of declaring dependencies between targets, this functionality is not utilized for testing.

## Usage

```cmake
dze_add_test(
    NAME <name>
    COMMAND <command>
    DEPENDS <dep1> [<dep2> ...])
```

Use `DEPENDS` for files or targets. Custom targets need to expose their file dependencies. (See
section 5 of https://samthursfield.wordpress.com/2015/11/21/cmake-dependencies-between-targets-and-files-and-custom-commands/ for details.) To achieve this, custom targets must be decorated with a property `dependent_files` that lists all file dependencies.

`dze_add_test` will internally add a target `${PROJECT_NAME}-run-test-<test name>` that can be used to run the test when any of its dependencies is updated. Furthermore, if there is a target `${PROJECT_NAME}-run-tests`, The single test target will be added as a dependency so multiple tests can be run by updating that target.
