# google-java-format-pre-commit

A performant [pre-commit] hook for [google-java-format] utilizing the GraalVM native binary without requiring a Java runtime.

This repo is based on [shellcheck-py] and maintains the exact same approach to provide the `google-java-format` native binary.

For more details, see the [Motivation](#motivation) section.

## Usage

This is only intended to be used with [pre-commit].

There are 2 built-in hooks:

- `google-java-format`: Follows the Google Style with 2-space indentation.
- `google-java-format-aosp`: Follows the AOSP Style with 4-space indentation.

Sample `.pre-commit-config.yaml` with the `google-java-format` hook:

```yaml
-   repo: https://github.com/natecrafts/google-java-format-pre-commit
    rev: v2.0.0
    hooks:
    -   id: google-java-format
```

Additional arguments for `google-java-format` can be specified through `args`
([documentation](https://pre-commit.com/#config-args)).

The following hook configuration formats with AOSP Style and skips formatting
Javadoc comments.

```yaml
-   repo: https://github.com/natecrafts/google-java-format-pre-commit
    rev: v2.0.0
    hooks:
    -   id: google-java-format-aosp
        args: [--skip-javadoc-formatting]
```

See [google-java-format configuration](https://github.com/google/google-java-format#configuration)
and [pre-commit] for more details.

## Motivation

The [google-java-format] project publishes a native binary built via GraalVM
besides the traditional JAR file. The native binary offers a few benefits over
the JAR file:

- The native binary is more performant, which is desirable for a code formatter.
- The native binary does not require a local JDK/JRE installation to run.

A native binary is published as part of each GitHub release of [google-java-format].

At the time this was written, [pre-commit] did not yet have a direct way to
download binaries and use them for hooks.
See issue https://github.com/pre-commit/pre-commit/issues/1453.

On the other hand, an alternative approach has been used by [shellcheck-py] to
create a hook for the `shellcheck` binary. The basic idea is making the
`shellcheck` binary available inside a Python environment and utilizing the
`python` language of [pre-commit]. This involves the use of the setuptools
plugin [setuptools-download] running at build time of the Python package to
download the binaries. Since the download happens during build time,
[pre-commit] is able to correctly cache the downloaded binaries for repeated
use of the hook. This repo is a fork of [shellcheck-py], adopting the same
approach and only adjusting where it is relevant for [google-java-format]
instead of `shellcheck`.

## Acknowledgements

- [google-java-format] by Google.
- [pre-commit] and [setuptools-download] by Anthony Sottile.
- [shellcheck-py] by Ryan Rhee and Anthony Sottile for the packaging and pre-commit hook pattern.

[pre-commit]: https://pre-commit.com
[google-java-format]: https://github.com/google/google-java-format
[shellcheck-py]: https://github.com/shellcheck-py/shellcheck-py
[setuptools-download]: https://github.com/asottile/setuptools-download
