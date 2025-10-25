# denv

**denv** ("dee-env") is a dependency-free Go package for parsing `.env` ("dot-env") files and manipulating their contents.
Originally developed for the job scheduler [Regular](https://github.com/dbohdan/regular), it is suitable for general use in Go projects.

```shell
go get dbohdan.com/denv
```

denv is mostly compatible with [GoDotEnv](https://github.com/joho/godotenv).
It is tested against test fixtures imported from GoDotEnv.
The one major difference is that attempting to substitute a nonexistent variable results in an error.
However, `.env` files have no formal specification, and differences in parsing around edge cases are to be expected.

## Features

- Parse `.env` files with quoted multiline values, backslash escape sequences, and comments
- Perform optional shell-style variable substitution
- Control substitution through quoting in the file: use double quotes or no quotes to enable it, single quotes to disable it
- Merge environments
- Convert between `[]string{"FOO=bar", ...}` and an environment map type

## Examples

Parse a `.env` file with variable substitution:

```go
content := strings.NewReader(`
# Set the base directory.
BASE=/opt
# Use substitution.
PATH=${BASE}/bin
`)

env, err := denv.Parse(content, true, nil)
// env = map[string]string{"BASE": "/opt", "PATH": "/opt/bin"}
```

Load from a file:

```go
// Use the contents of os.Environ for substitution.
substEnv := denv.OS()
env, err := denv.Load(".env", true, substEnv)
```

Convert environment strings:

```go
// From string slice to map.
env := denv.EnvFromStrings([]string{"FOO=bar", "BAZ=qux"})

// Back to a string slice.
strings := env.Strings()
```

Merge multiple environments:

```go
merged := denv.Merge(env1, env2, env3)
```

## License

MIT.
See the file [`LICENSE`](LICENSE).
