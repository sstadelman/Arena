# SPMPlaygrounds

SPMPlaygrounds is a macOS command line tool to create an Xcode project with a Swift Playground that's readily set up to use a Swift Package Manager library. You can reference both Github and local repositories. The latter is especially useful to spin up a Playground while working on a library.

```
 ~  spm-playground --help
OVERVIEW: Creates an Xcode project with a Playground and one or more SPM libraries imported and ready for use.

USAGE: spm-playground [options]

OPTIONS:
  --deps, -d         Dependency url(s) and (optionally) version specification [default: []]
  --force, -f        Overwrite existing file/directory [default: false]
  --help, -h         Display available options [default: false]
  --libs, -l         Names of libraries to import (inferred if not provided) [default: []]
  --name, -n         Name of directory and Xcode project [default: SPM-Playground]
  --outputdir, -o    Directory where project folder should be saved [default: /Users/sas/Projects/SPMPlayground]
  --platform, -p     Platform for Playground (one of 'macos', 'ios', 'tvos') [default: macos]
  --version, -v      Display tool version [default: false]
```

## Examples

### Import Github repository

```
 ~  spm-playground -d https://github.com/johnsundell/plot
🔧  resolving package dependencies
📔  libraries found: Plot
✅  created project in folder 'SPM-Playground'
```

### Import local repository

```
spm-playground -d ~/Projects/Parser
🔧  resolving package dependencies
📔  libraries found: Parser
✅  created project in folder 'SPM-Playground'
```

### Import both

```
spm-playground -d ~/Projects/Parser https://github.com/johnsundell/plot
🔧  resolving package dependencies
📔  libraries found: Parser, Plot
✅  created project in folder 'SPM-Playground'
```

## Specifying versions

In case you want to fetch a particular revision, range of revisions, or branch, you can use a syntax similar to the one used in a `Package.swift` file. Here's what's supported and the corresponding package dependecy that it will create in the generated project:

- `-d https://github.com/johnsundell/plot@0.3.0`
  
  → `.package(url: "https://github.com/johnsundell/plot", .exact("0.3.0"))`

- `-d https://github.com/johnsundell/plot@from:0.1.0`
  
  → `.package(url: "https://github.com/johnsundell/plot", from: "0.1.0")`

- `-d "https://github.com/johnsundell/plot@0.1.0..<4.0.0"`

  → `.package(url: "https://github.com/johnsundell/plot", "0.1.0"..<"4.0.0")`

- `-d https://github.com/johnsundell/plot@0.1.0...4.0.0"` 

  → `.package(url: "https://github.com/johnsundell/plot", "0.1.0"..<"4.0.1")`

- `-d https://github.com/johnsundell/plot@branch:master` 

  → `.package(url: "https://github.com/johnsundell/plot", .branch("master"))`

- `-d https://github.com/johnsundell/plot@revision:2e5574972f83bc5cdea59662986e701b86137642` 

  → `.package(url: "https://github.com/johnsundell/plot", .revision("2e5574972f83bc5cdea59662986e701b86137642"))`

Make sure to properly quote the URL if you are using the `..<` range operator.

## How to build and install

You can build and install `spm-playground` via the included `Makefile` by running:

```
make install
```

This will copy the binary `spm-playground` to `/usr/local/bin`.

## Compatibility

`spm-playground` was built and tested on macOS 10.15 Catalina using Swift 5.1.3. It should work on other versions of macOS and Swift as well.
