# CMake guidelines

### Contents

- [Notation](#notation)
- [Project structure](#project-structure)
- [Naming conventions](#naming-conventions)
- [Examples](#examples)


## Notation

- `Project`: repository name of a standalone C++ project, e.g., `CppProjectTemplate`
- `Super_Sub`: repository name of a C++ project `Sub` within a larger super project
  `Super`, e.g., `Bullseye_BallisticSimulator`


## Project structure

In general, we follow the structure of the [C++ project
template](https://github.com/fantana21/CppProjectTemplate). Some of the most important
points as well as additional considerations and exceptions for larger projects are
explained below.

- The C++ source code goes into `Project/` or `Super/Sub/`, not `src/` or something like
  that.
- We do *not* split the headers and source files into separate folders.
- Larger projects should not have all code in a single folder like the C++ project
  template. It should be organized into components, with each component implemented as a
  separate library target in its own subfolder under `Project/` or `Super/Sub/`.
- If the project exports a library, it's include paths must be nice. Therefore, the
  primary library code goes directly into `Project/` or `Super/Sub/` such that users can
  include headers as `<Project/Header.hpp>` or `<Super/Sub/Header.hpp>`.
- If a project that exports a library also contains an executable, the executable's code
  should be in a separate folder which is on the same level as the library folder, e.g.,
  `ProjectApp/` or `Super/SubApp/`.


## Naming conventions

In general, we use the following cases when naming identifiers.

| Type            | Case          |
| --------------- | ------------- |
| file/folder     | PascalCase    |
| target          | PascalCase    |
| cache variable  | SHOUTING_CASE |
| normal variable | snake_case    |
| function        | snake_case    |
| macro           | snake_case    |

For some names we have more specific rules.

- CMake project name = `Project` or `Super_Sub`
- Targets get a project-specific prefix: `Project_` or `Super_`
- Library targets get an alias with prefix `Project::` or `Super::`
- Target output name = target name without prefix
- CMake cache variables are prefixed with the project name: `Project_` or `Super_Sub_`

Test targets follow similar but different naming conventions to prevent name clashes.

- Target name prefix = `ProjectTests_` or `SuperTests_`
- Target output name = target name without prefix + `Test`, i.e.,
  `ProjectTests_Component` → `ComponentTest`

If the project should be used by others, it needs to provide a proper CMake package
([config-file
package](https://cmake.org/cmake/help/latest/manual/cmake-packages.7.html#config-file-packages))
and follow additional conventions.

- Package name = `Project` or `Super_Sub`
- Target export name = target name without prefix
- Export namespace = `Project::` or `Super::`

The last two points ensure that the targets are exported with the same names as the
aliases defined within the project. (Actually, the export names and namespace are the
reason for the alias targets and prefixes and not the other way around.)


## Examples

| Name                               | Standalone | Components          | Size  |
| ---------------------------------- | ---------- | ------------------- | ----- |
| [`QuantNd`][1]                     | yes        | library             | small |
| [`Bullseye_BallisticSimulator`][1] | no         | library, executable | small |
| [`Bullseye_FireControlSystem`][1]  | no         | executable          | large |

[1]: https://github.com/fantana21/QuantNd
[2]: https://github.com/fantana21/Bullseye_BallisticSimulator
[3]: https://github.com/fantana21/Bullseye_FireControlSystem


### `QuantNd`

The C++ code for the library is in `QuantNd/`.

| Entity                  | Name/value                          |
| :---------------------- | :---------------------------------- |
| CMake project name      | `QuantNd`                           |
| Library target          | `QuantNd_QuantNd`                   |
| Library alias           | `QuantNd::QuantNd`                  |
| Library output name     | `QuantNd`                           |
| Cache variables/options | `QuantNd_ENABLE_CCACHE`, ...        |
| Package name            | `QuantNd`                           |
| Library export name     | `QuantNd`                           |
| Export namespace        | `QuantNd::`                         |
| Include paths           | `<QuantNd/QuantityMatrix.hpp>`, ... |

Users of the library can do:

~~~cmake
find_package(QuandNd CONFIG REQUIRED)
target_link_libraries(MyApp PRIVATE QuandNd::QuandNd)
~~~


### `Bullseye_BallisticSimulator`

The C++ code for the library is in `Bullseye/BallisticSimulator/`, the code for the
executable is in `Bullseye/BallisticSimulatorApp/`.

| Entity                  | Name/value                                       |
| :---------------------- | :----------------------------------------------- |
| CMake project name      | `Bullseye_BallisticSimulator`                    |
| Library target          | `Bullseye_BallisticSimulator`                    |
| Library alias           | `Bullseye::BallisticSimulator`                   |
| Library output name     | `BallisticSimulator`                             |
| Executable target       | `Bullseye_BallisticSimulatorApp`                 |
| Executable output name  | `BallisticSimulatorApp`                          |
| Cache variables/options | `Bullseye_BallisticSimulator_ENABLE_CCACHE`, ... |
| Package name            | `Bullseye_BallisticSimulator`                    |
| Library export name     | `BallisticSimulator`                             |
| Export namespace        | `Bullseye::`                                     |
| Include paths           | `<Bullseye/BallisticSimulator/Bullet.hpp>`, ...  |

Users of the library can do:

~~~cmake
find_package(Bullseye_BallisticSimulator CONFIG REQUIRED)
target_link_libraries(MyApp PRIVATE Bullseye::BallisticSimulator)
~~~


### `Bullseye_FireControlSystem`

The C++ code for the executable is in `Bullseye/FireControlSystem/`. It is organized into
multiple components, with each component implemented as a separate library target in its
own subfolder. None of the libraries are intended to be used outside the project, so they
are not exported.

| Entity                  | Name/value                                             |
| :---------------------- | :----------------------------------------------------- |
| CMake project name      | `Bullseye_FireControlSystem`                           |
| Executable target       | `Bullseye_FireControlSystem`                           |
| Executable output name  | `FireControlSystem`                                    |
| Library targets         | `Bullseye_Sensors`, `Bullseye_MotionController`, ...   |
| Library aliases         | `Bullseye::Sensors`, `Bullseye::MotionController`, ... |
| Cache variables/options | `Bullseye_FireControlSystem_ENABLE_CCACHE`, ...        |
| Include paths           | `<Bullseye/FireControlSystem/Sensors/Camera.hpp>`, ... |
