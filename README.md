# EDIB mix task

[![Hex.pm package version](https://img.shields.io/hexpm/v/edib.svg?style=flat-square)](https://hex.pm/packages/edib)
[![Hex.pm package docs](https://img.shields.io/badge/hex-docs-orange.svg?style=flat-square)](http://hexdocs.pm/edib/)
[![Hex.pm package license](https://img.shields.io/hexpm/l/edib.svg?style=flat-square)](https://github.com/edib-tool/mix-edib/blob/master/LICENSE)
[![Build Status (master)](https://img.shields.io/travis/edib-tool/mix-edib/master.svg?style=flat-square)](https://travis-ci.org/edib-tool/mix-edib)
[![Coverage Status (master)](https://img.shields.io/coveralls/edib-tool/mix-edib/master.svg?style=flat-square)](https://coveralls.io/r/edib-tool/mix-edib)
[![Deps Status](https://beta.hexfaktor.org/badge/all/github/edib-tool/mix-edib.svg?style=flat-square)](https://beta.hexfaktor.org/github/edib-tool/mix-edib)
[![Inline docs](http://inch-ci.org/github/edib-tool/mix-edib.svg?branch=master&style=flat-square)](http://inch-ci.org/github/edib-tool/mix-edib)

A mix task for [EDIB (elixir docker image builder)](https://github.com/edib-tool/elixir-docker-image-builder).

<!--
  TOC generaged with doctoc: `npm install -g doctoc`

    $ doctoc README.md --github --maxlevel 4 --title '## TOC'

-->
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
## TOC

- [Installation](#installation)
- [Usage](#usage)
- [Help](#help)
- [Options](#options)
  - [Name, prefix, tag](#name-prefix-tag)
  - [Release strip and zip (EXPERIMENTAL)](#release-strip-and-zip-experimental)
  - [Silent mode (quiet mode)](#silent-mode-quiet-mode)
  - [Volume mappings](#volume-mappings)
  - [Docker related](#docker-related)
  - [Developer options](#developer-options)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
<!-- moduledoc: Mix.Tasks.Edib -->
EDIB creates a docker image of your application release.

## Installation

Just run this and confirm:

    mix archive.install hex edib

Don't forget to add `distillery` to your project:

    defp deps do
      [
        {:distillery, "~> 1.1"},
      ]
    end

## Usage

    mix edib

mix-edib will use the MIX_ENV environment variable to build the image.

    MIX_ENV=staging mix edib

**WARNING:** If `MIX_ENV` is not set EDIB will build the image for the `prod` environment.

## Help

    mix help edib

## Options

### Name, prefix, tag

Override the (repository) name of the docker image

    mix edib --name <NAME>
    mix edib -n <NAME>

Set only a specific prefix for the docker image name (default: local)

    mix edib --prefix <PREFIX>
    mix edib -p <PREFIX>

Set a specific tag for the docker image

    mix edib --tag <TAG>
    mix edib -t <TAG>

If `--name` and `--prefix` are given, the name option takes precedence
(prefix will be ignored).

### Release strip and zip (EXPERIMENTAL)

Following options use <https://github.com/ntrepid8/ex_strip_zip> in the
edib-tool build environment.

All .beam files in a release can be stripped (mostly of debug information):

    mix edib --strip
    mix edib -x

More technical information about stripping:
<http://erlang.org/doc/man/beam_lib.html#strip-1>

All OTP applications can be bundled into archives (.ez files):

    mix edib --zip
    mix edib -z

**WARNING:** Do not use this if you have NIFs in your codebase or dependencies.

More technical information about "Loading of Code From Archive Files":
<http://erlang.org/doc/man/code.html#id104826>

### Silent mode (quiet mode)

Silence build output of EDIB (will be logged to `.edib.log` instead)

    mix edib --silent
    mix edib -s

### Volume mappings

Map additional volumes for use while building the release

    mix edib --mapping <FROM>:<TO>[:<OPTION>]
    mix edib -m <FROM>:<TO>[:<OPTION>]

For common cases there are some mapping shorthands:

Include the host user's SSH keys for private GitHub repositories

    mix edib --ssh-keys

Include host user's .hex/packages cache

Can improve build times when the host's .hex cache is available for
every build run (tip for Travis CI: use their directory caching)

    mix edib --hex

Include host user's .npm package cache

Can improve build times when the host's .npm cache is available for
every build run (tip for Travis CI: use their directory caching)

    mix edib --npm

### Docker related

Docker flags (mostly for debug purposes):

Run the build step privileged

    mix edib --privileged

Do not remove intermediate containers on build runs

    mix edib --no-rm

### Developer options

Select edib-tool docker image (complete repo + version)

    mix edib --edib edib/edib-tool:1.5.2
    mix edib -e edib/edib-tool:1.5.2
<!-- endmoduledoc: Mix.Tasks.Edib -->
