# Dart Package Manager: `pub`

This document covers Dart’s package manager, `pub`, detailing how to configure dependencies, manage packages, and publish your own libraries. It includes examples of common commands and best practices.

---

## 1. Overview of `pub`

- **`pub`** is the official package manager for Dart projects.
- Manages dependencies, version constraints, package resolution, and publishing to [pub.dev](https://pub.dev).
- Invoked via the `dart pub` command or `flutter pub` in Flutter projects.

---

## 2. The `pubspec.yaml` File

The central configuration for a Dart package.

```yaml
name: my_package
version: 1.0.0
description: A short description of my package.
environment:
  sdk: '>=2.19.0 <3.0.0'

dependencies:
  http: ^0.14.0
  path: any

dev_dependencies:
  test: ^1.20.0

dependency_overrides:
  logging: 1.0.2

executables:
  my_cli: bin/cli.dart
```

- **`dependencies`**: packages required at runtime.
- **`dev_dependencies`**: packages used for development (e.g., testing).
- **`dependency_overrides`**: force a specific version across your app.
- **`executables`**: define CLI entry points for global activation.

---

## 3. Version Constraints

Specify allowed versions with:

- **Caret syntax** `^1.2.3`: allows `>=1.2.3 <2.0.0`.
- **Range syntax** `>=1.0.0 <1.5.0`.
- **Wildcard** `1.2.x`: allows `>=1.2.0 <1.3.0`.
- **Any** `any`: no constraint, use latest compatible.

Example:

```yaml
dependencies:
  json_annotation: '>=4.0.1 <5.0.0'
```

---

## 4. Common `pub` Commands

| Command                         | Description                                                   |
|---------------------------------|---------------------------------------------------------------|
| `dart pub get`                  | Fetches dependencies listed in `pubspec.yaml`.                |
| `dart pub upgrade`              | Upgrades dependencies to the latest allowed versions.         |
| `dart pub outdated`             | Shows outdated dependencies.                                  |
| `dart pub deps`                 | Displays a dependency graph.                                  |
| `dart pub cache repair`         | Repairs or reinstalls cached packages.                        |
| `dart pub global activate <pkg>`| Installs a package globally for CLI use.                      |
| `dart pub global run <exe>`     | Runs an executable from a globally activated package.         |
| `dart pub publish`              | Publishes the package to pub.dev (requires authentication).   |

> Note: In Flutter projects, prefix commands with `flutter` (e.g., `flutter pub get`).

---

## 5. Dependency Management Workflows

### 5.1 Adding a Dependency

```bash
dart pub add logger
# Adds to pubspec.yaml and runs pub get
```

### 5.2 Removing a Dependency

```bash
dart pub remove logger
```

### 5.3 Upgrading a Single Package

```bash
dart pub upgrade http
```

### 5.4 Checking for Outdated Packages

```bash
dart pub outdated
```

Output shows current, upgradable, and resolvable versions.

---

## 6. Working with Global Packages

- Install CLI tools globally:
  ```bash
dart pub global activate stagehand
```
- List global packages:
  ```bash
dart pub global list
```
- Run executables:
  ```bash
dart pub global run stagehand web-simple
```

Add `~/.pub-cache/bin` to your PATH to run tools directly.

---

## 7. Publishing Packages

1. **Ensure compliance**:
   - `pubspec.yaml` fields (`name`, `version`, `description`, `homepage`, `repository`, `authors`).
   - Include a `LICENSE` file.
   - Provide documentation in `README.md` and API docs via Dartdoc comments.
2. **Validate**:
   ```bash
dart pub publish --dry-run
```
3. **Publish**:
   ```bash
dart pub publish
y```

Your package appears on [pub.dev](https://pub.dev).

---

## 8. Configuring the Pub Cache

- Cache location: `~/.pub-cache` by default.
- Clear or repair cache:
  ```bash
dart pub cache repair
```
- Pre-cache dependencies in CI:
  ```bash
dart pub cache add <package_name>
```

---

## 9. Best Practices

- **Lock file**: Commit `pubspec.lock` for applications; omit for libraries.
- **Semantic versioning**: Follow SemVer for package versions.
- **Minimal dependencies**: Only include necessary packages.
- **Automate checks**: Use CI to run `dart pub get`, `dart pub outdated`, and tests.

---

## 10. Further Resources

- [Dependecy management](https://dart.dev/tools/pub/dependencies)
- [Publishing packages](https://dart.dev/tools/pub/publishing)
- [pub.dev documentation](https://dart.dev/tools/pub)

---

*End of Dart Package Manager: `pub`*

