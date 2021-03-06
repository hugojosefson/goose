# Goose, the dumb file generator

![goose](goose-small.png)

Goose, the dumb file generator. Goose takes a file structure and replaces names
with values. The values can be converted into several formats, `snakecase`,
`camelcase`, `dromedarcase`, `dasherized`, `spaceseparated` and `titlecase`.

The file structure is created exactly as the template file structure with the
keys replaced with the values. It is a tool similar to Thor and Yeoman, but
dumber, because dumber is simpler.

## Installation

### Homebrew Installation on OS X

```
$ brew tap andersjanmyr/tap
$ brew install goose
```

### Installation without Homebrew

`goose` is a single binary. Install it by right-clicking and `Save as...` or with
`curl`.

### Links

* [OS X](https://github.com/andersjanmyr/goose/releases/download/v1.3.5/goose-osx)
* [Linux](https://github.com/andersjanmyr/goose/releases/download/v1.3.5/goose-linux)
* [Windows](https://github.com/andersjanmyr/goose/releases/download/v1.3.5/goose.exe)
* [Bash completion](https://raw.githubusercontent.com/andersjanmyr/goose/v1.3.5/goose_completion.sh)

### Curl

```
# OS X
$ curl -L https://github.com/andersjanmyr/goose/releases/download/v1.3.5/goose-osx \
  > /usr/local/bin/goose

# Linux
$ curl -L https://github.com/andersjanmyr/goose/releases/download/v1.3.5/goose-linux \
  > /usr/local/bin/goose

# Make executable
$ chmod a+x /usr/local/bin/goose

# Bash Completion
$ curl -L https://raw.githubusercontent.com/andersjanmyr/goose/master/goose_completion.sh \
  > /your/bash_completion/dir/
```

## Goose Templates

A collection of Goose templates can be found in a separate repository,
[goose-templates](https://github.com/andersjanmyr/goose-templates). Fork it and
clone it or download the latest version as a
[zip archive](https://github.com/andersjanmyr/goose-templates/archive/master.zip).

It is recommended to install the templates into `~/.goose`

```
$ git clone git@github.com:andersjanmyr/goose-templates.git ~/.goose
```

## Usage

```
$ goose --verbose --data 'name=string,title=date' seq-model articles
OPTIONS:
verbose: true
force: false
interactive: false
template: seq-model
name: articles
templateDir: /Users/andersjanmyr/.goose
outputDir: .
data: map[NAME:articles TITLE:date DATA:map[title:date name:string]]
Creating dir .
Generating file /db/migrations/articles.js
```

As you can see from the data option above, the values given to the `--data`
option are available to the template as upper cased and as a DATA map with the
values as they where given.


```
$ goose
Usage: main [options] <template> <name>
  -data=map[]: Extra data, format: key1=val1,key2=val2 (keys are upcased)
  -force=false: Force create files if they exist
  -help=false: Show help text
  -interactive=false: Ask before creating anything
  -outputdir=".": Output directory
  -templatedir="/Users/andersjanmyr/.goose": Directory where templates are stored
  -verbose=false: Be verbose

Available functions in templates are (filename suffixes in parenthesis):
	boacase (.bc)           - MY_BEAUTIFUL_TAPIR
	camelcase (.cc)         - MyBeautifulTapir
	dasherized (.da)        - my-beautiful-tapir
	dromedarcase (.dc)      - myBeautifulTapir
	snakecase (.sc)         - my_beautiful_tapir
	spaceseparated (.ss)    - my beautiful tapir
	titlecase (.tc)         - My Beautiful Tapir
	lowercaseletters (.ll)  - mybeautifultapir
```

### Template Name

If `<template>` is a `git clone`able URL it is cloned and used as the template,
effectively ignoring the `-templatedir` parameter.

## Template Files

The template files are normal go template files and support one property
`.NAME` and functions `snakecase` (`my_app`), `dasherized` (`my-app`),
`camelcase` (`MyApp`), `dromedarcase` (`myApp`), `spaceseparated` (`my app`),
`title case` (`My App`), `lowercaseletters` (`myapp`).

```go
// NAME.dc.go
package {{dromedarcase .NAME}}

func {{camelcase .NAME}}() string {
        return "{{dromedarcase .NAME}}"
}
```

The same functions that are available inside the templates can also be used in
the filenames (and directory names), in this case they are called `NAME.sc.go`,
`NAME.da.go`, `NAME.cc.go`, `NAME.dc.go`, `NAME.ss.go`, and `NAME.tc.go`.

```
# Example, simple structure
$ tree ~/.goose/go
go
├── NAME.dc.go
└── NAME.dc_test.go
```

```
# Example, generated simple structure
$ goose go tapir
$ tree .
.
├── tapir.go
└── tapir_test.go

```

```
# Example, bigger structure
$ tree ~/.goose/lambda/
lambda/
└── NAME.da
    ├── add-event-source.sh
    ├── create-roles.sh
    ├── generate-events.js
    ├── iam
    │   ├── lambda-assumerole-policy.json
    │   └── s3-assumerole-policy.json
    ├── invoke-lambda.sh
    ├── NAME.da.js
    ├── package.json
    └── upload-lambda.sh
```

```
# Example, generated bigger structure
$ goose lambda cool-micro-service
$ tree cool-micro-service
cool-micro-service/
├── add-event-source.sh
├── cool-micro-service.js
├── create-roles.sh
├── generate-events.js
├── iam
│   ├── lambda-assumerole-policy.json
│   └── s3-assumerole-policy.json
├── invoke-lambda.sh
├── package.json
└── upload-lambda.sh
```

## List of Functions

* `boacase (bc)` - `MY_BEAUTIFUL_TAPIR`
* `camelcase (cc)` - `MyBeautifulTapir`
* `dasherized (da)` - `my-beautiful-tapir`
* `dromedarcase (dc)` - `myBeautifulTapir`
* `snakecase (sc)` - `my_beautiful_tapir`
* `spaceseparated (ss)` - `my beautiful tapir`
* `titlecase (tc)` - `My Beautiful Tapir`
* `lowercaseletters (ll)` - `mybeautifultapir`


## Release Notes

A list of changes are in the [RELEASE_NOTES](RELEASE_NOTES.md).

