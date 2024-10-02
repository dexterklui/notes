---
date: 2024-09-26 (Thu)
---

# Maven

## Build Life Cycle

See
[Introduction to the Build Lifecycle - Apache Maven Project](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference)

There are three different built-in life cycles:

- `default` handles project deployment
- `clean` handles project cleaning
- `site` handles the creation of your project's web site

Each life cycle is made up of phases. For default, generally the following
phases are executed:

- `validate` - validate the project is correct and all necessary information is
  available
- `compile` - compile the source code of the project
- `test` - test the compiled source code
- `package` - package the compiled code and all necessary resources into its
  distribution format, e.g. a JAR
- `verify` - run any checks on results of integration tests to ensure quality
- `install` - install the package into the local repository (default inside
  `.m2` folder), for use as a dependency in other projects locally
- `deploy` - done in an integration or release environment, copies the final
  package to the remote repository for sharing with other developers and
  projects

## Plugin Goals

A build phase is made up of plugin goals.

While a phase is responsible for a specific step in the life cycle, the manner
in which it carries out those responsibilities may vary, and this is done by
declaring the **_plugin goals_** bound to those build phases.

A plugin goal represents a specific task (finer than a build phase) which
contributes to the building and managing of a project. It may be bound to zero
or more build phases.

A build phase can also have zero or more goals bound to it. If a build phase has
no goals bound to it, that build phase will not execute. Otherwise it will
execute all those goals.

## Life cycle and plugin goals binding

The default bindings depend on the **_packaging_** type of the maven project.
Which is set by `<packaging>` tag in the `pom.xml` file. The default packaging
type is `jar`

### Default Bindings

For all packaging:

- `clean` phase: `clean:clean` from the `maven-clean-plugin`

For `jar` packaging:

| Phase                    | plugin:goal               |
| ------------------------ | ------------------------- |
| `process-resources`      | `resources:resources`     |
| `compile`                | `compiler:compile`        |
| `process-test-resources` | `resources:testResources` |
| `test-compile`           | `compiler:testCompile`    |
| `test`                   | `surefire:test`           |
| `package`                | `jar:jar`                 |
| `install`                | `install:install`         |
| `deploy`                 | `deploy:deploy`           |

For packagings `ejb`, `ejb3`, `par`, `rar`, `war`, they have similar default
bindings. The only difference is in the binding for `package` phase, which has
the bounding `rar:rar`, `war:war`, etc.

## Executing with Maven

- `mvn <life-cycle>` runs the specified life cycle
- `mvn <phase>` runs a corresponding life cycle up to the specified phase
- `mvn <plugin>:<goal>` runs a specific plugin goal

E.g. `mvn clean install` will run the `clean` life cycle to clear all previously
compiled and packaged files. Then it runs the `default` life cycle up to the
`install` phase.

E.g. `mvn install:install` will run the `install` goal without running any other
goals or build phases.

## 🧭 Navigation

- [🔼 Back to top](#maven)
- [◀️ Back](java.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
