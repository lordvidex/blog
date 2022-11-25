---
title: Creating CLI in Go
date: 2022-10-16T20:29:22+03:00
draft: false
weight: 1
description: A deep dive into the gomeasure CLI tool (early release)
cover:
  image: images/go-calculate.webp
  alt: gomeasure image
  relative: true
  side: right
  hidden: false
author:
  - Evans Owamoyo
  - Ahmed Helali
categories:
  - Go
tags:
  - CLI
---
## Intro 🚪
Go is an open source programming language supported by Google. It has gained it's grounds 
## The Journey 🚲
 <!-- I learnt the basics of Go in roughly 3 days, and I enjoyed the experience 😋 because of the interesting nature of the language. I also shared how I learn programming languages fast in this [article](https://evansowamoyo.com/posts/learning-programming-languages-fast/). 
 After mastering the syntax, I worked on some [open source projects](https://github.com/lordvidex?tab=repositories&language=go)   I decided to create a CLI tool in Go.
 -->
<!-- Hunting for project ideas -->
I was hunting for project ideas in go when at work, there was a need to calculate the weight (files count) of directories in a project.
Hence, I wrote a simple Go script which evolved into the `gomeasure` CLI tool.
## Why I built with Go? 🤔
I chose Go because of the following reasons:
### I was learning go at the time
I've developed the habit of learning at least one new programming language every year. I started learning Go in 2022 and I wanted to build something with it.
### Go has a rich standard library and inbuilt tools for building fast elegant CLIs.
The `flag`, `os` and `fmt` packages are enough for building a simple CLI tool. This provides flag parsing out of the box compared to other languages like Java where I would have parsed arguments manually. Anyways, I extended these packages with the `github.com/spf13/cobra` package which made building even easier.
### Collaboration and community.
I learnt and built the tool with a friend, [Ahmed Helali](https://escalopa.live/). We both have a passion for open source and we wanted to build something together.
### Cross-platform support (Windows, Linux, Mac)
Go is a compiled language and it compiles to a single binary file which can be run on any platform. This is unlike interpreted languages like Python and JavaScript which require a runtime environment to run. This makes Go a good choice for building CLIs. I built the tool for various platforms in a CI/CD environment and it worked on all platforms without issues.
<!-- - I started with the website and it had a lot of examples and tutorials
- I always use exercism to learn fundamentals
- I  -->
## Structure of a CLI program 🏗
As developers, we use CLI programs every day ranging from git, docker, kubectl, etc. A typical CLI program has the following structure:  
`<program> [command] [arguments] [flags]` e.g.  
`gomeasure line /path/to/file -v`   
I employed the following model for the `gomeasure` CLI tool:
- In the docs, `<>` means a required argument and `[]` means an optional argument.
- `[command]` is a sub-command of the program. e.g. `line` in `gomeasure line` is a sub-command of `gomeasure`.
- sub-commands have required arguments and optional flags. e.g. `gomeasure line /path/to/file -v` has a required argument `/path/to/file` and an optional flag `-v`.  
*Here, it is obvious that the **arguments** are necessary for the subcommand to carry out the expected action while the **flags** tells it how to.*
- `gomeasure` has a `help` sub-command which displays the help message for the program. e.g. `gomeasure help` displays the help message for the program. The help message is also accessible from the `--help` flag. e.g. `gomeasure line --help` displays the help message for the `line` sub-command.
- Flags or options that have a single dash `-` are short flags. e.g. `-v` is a short flag and is equivalent to `--verbose`.
- Short flags can be combined. e.g. `-v -s` is equivalent to `-vs`.
- Flags or options that have a double dash `--` are long flags. e.g. `--verbose` is a long flag.

## Introduction to Cobra 🐍
[Cobra](https://github.com/spf13/cobra) is a package built by [Steve Francia](https://github.com/spf13) for building powerful modern CLI applications in Go. It provides a simple interface to create powerful modern CLI interfaces and it is used in production grade softwares like Kubernetes, Hugo and Github CLI, to name a few. Cobra provides a generator to create a CLI skeleton which can be customized to suit your needs. I used the generator to create the `gomeasure` CLI tool.
> Cobra out of the box use the [structure]({{<ref "#structure-of-a-cli-program-">}}) described above.  
## Creating *gomeasure* with Cobra and Go 🛠
### Project structure
The project structure is as follows:
```
.
├── LICENSE
├── README.md
├── bin
│   └── gomeasure
├── .goreleaser.yaml
├── cmd
│   ├── file.go
│   ├── line.go
│   └── root.go
├── dist/
├── go.mod
├── go.sum
├── main.go
├── pkg
│   ├── runner.go
│   └── runner_test.go
```
- `LICENSE` is the license file.
- `README.md` is the readme file.
- `bin` is the directory where the compiled binary is stored.
- `cmd` is the directory where the CLI commands are stored.
- `dist` is the directory where the compiled binaries for various platforms are stored.
- `go.mod` is the go module file.
- `go.sum` is the go module checksum file.
- `main.go` is the entry point of the program.
- `pkg` is the directory that contains the business logic of the software.
- `.goreleaser.yaml` is the goreleaser configuration file.

### Cobra and the cmd/ directory
The scope of cobra is limited to the `cmd/` directory because this is where each sub-command is defined and flags are parsed.
* The most important class in the `cobra` package is the `cobra.Command` class. It is used to define a sub-command and its flags. An example of how we used it to define the `file` sub-command is shown below:
```go
var fileCmd = &cobra.Command{
	Use: "file <directory>",

	Short: "processes the number of files in a directory",
	Long:  `gomeasure file processes and returns the number of files in a directory / project.`,
	Run: func(cmd *cobra.Command, args []string) {
		// ...
	},
}
```

* Cobra also helps to parse flags and it automatically resolves conflicts between flags. An example of how we used it to parse the `--verbose` flag is shown below:
```go 
// cmd/root.go
rootCmd.PersistentFlags().BoolVarP(&isVerbose, "verbose", "v", false, "--desc--")
```
> You can check out the cobra documentation at https://github.com/spf13/cobra.

### The runner.go file
The `runner.go` file contains the business logic of the program. It contains the `Runner` struct which contains fields that helps to specify what the program has to do.  
This file also contains the `Result` struct which is returned from the `Run` method of the `Runner` struct. The `Result` struct contains the result of the program execution.

### Deployment and CI/CD with Github Actions
I used Github Actions to build and deploy the program. The workflow is as follows:
- The workflow is triggered when a new release is created.
- The workflow builds the program for various platforms using goreleaser.
The `.goreleaser.yaml file contains instructions on how and where to deploy releases.
- The workflow then creates a new release and uploads the compiled binaries to the github releases section and on brew (for macOS).

## Roadmap for *gomeasure* 🛣
### More subcommands 🤖
I plan to improve the tool by adding more sub-commands and more flags to the existing sub-commands to make it more useful.
### Unit Testing omg 🥵
I plan to add unit tests to the ***.go** files. It is actually a highly recommended practice to write unit tests in the Go community.
### Docker image 🐳
I plan to create a docker image release for the tool so that it can be used in a docker container.

--- 
## Conclusion
I've made the project open source and I would love to see contributions from the community.

Don't forget to star🌟 [the repository](https://github.com/lordvidex/gomeasure) on Github if you found **gomeasure** useful.

## Helpful Links
- [gomeasure on Github](https://github.com/lordvidex/gomeasure)
- [gomeasure on pkg.go.dev](https://pkg.go.dev/github.com/lordvidex/gomeasure)
- [Go website](https://go.dev/)
- [Cobra Github Repository](https://github.com/spf13/cobra/)
- [Interesting read on CLI](https://dev.to/paulasantamaria/command-line-interfaces-structure-syntax-2533)
- [The Go programming language and it's usecases](https://go.dev/solutions/#use-cases)