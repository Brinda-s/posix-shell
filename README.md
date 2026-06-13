# posix-shell

![Java](https://img.shields.io/badge/Java-17-007396?logo=openjdk&logoColor=white)
![Build](https://img.shields.io/badge/build-Maven-C71A36?logo=apachemaven&logoColor=white)

A POSIX-compliant Unix shell built from scratch in Java — no shell libraries, just the JDK. It parses raw input, runs builtins and external programs, chains commands through pipelines, redirects I/O, expands environment variables, and manages concurrent background jobs.

## Features

- **Command parsing** — lexical tokenization and syntax validation of raw input, including single/double quoting and backslash escaping
- **18 builtin commands** — `cd`, `pwd`, `echo`, `export`, `type`, `exit`, and more
- **External programs** — resolves and executes binaries from `PATH`
- **Pipelines** — unbounded command pipelines (`cmd1 | cmd2 | cmd3 | ...`)
- **I/O redirection** — `>`, `>>`, `<`, and stderr (`2>`) redirection
- **Background jobs** — `&` execution, handling 100+ concurrent jobs via a thread pool
- **Environment variables** — assignment and `$VAR` expansion

## Tech stack

Java · Maven · `java.util.concurrent.ExecutorService` for concurrent job management

## Getting started

### Prerequisites
- JDK 17 or later
- Maven

### Build and run
```sh
mvn -B package
./your_program.sh
```

This starts an interactive REPL. Type commands as you would in any shell:

```sh
$ echo hello | wc -c
6

$ ls -la > files.txt

$ sleep 5 &
[1] 12345

$ export NAME=brinda && echo $NAME
brinda
```

## How it works

```
input → tokenizer → parser → executor → job manager
```

1. **REPL loop** reads a line of input.
2. **Tokenizer** splits it into tokens, applying quoting and escape rules.
3. **Parser** builds a command structure, resolving pipes, redirections, and background flags.
4. **Executor** dispatches builtins directly or spawns external processes, wiring up stdin/stdout/stderr for pipelines and redirection.
5. **Job manager** runs background jobs on an `ExecutorService` thread pool and tracks their lifecycle.

## Project structure

```
src/main/java/    # shell implementation (entry point: Main.java)
pom.xml           # Maven build
```
