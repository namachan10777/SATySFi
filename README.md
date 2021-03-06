![logo1](https://raw.githubusercontent.com/wiki/gfngfn/SATySFi/img/satysfi-logo.png)

[![Build Status](https://travis-ci.org/gfngfn/SATySFi.svg?branch=master)](https://travis-ci.org/gfngfn/SATySFi)

[日本語版 README はこちら](https://github.com/gfngfn/SATySFi/blob/master/README-ja.md)

## Summary of SATySFi

*SATySFi* (pronounced in the same way as the verb “satisfy” in English) is a new typesetting system equipped with a statically-typed, functional programming language. It consists mainly of two “layers” ― the text layer and the program layer. The former is for writing documents in LaTeX-like syntax. The latter, which has OCaml-like syntax, is for defining functions and commands. SATySFi enables you to write documents markuped with flexible commands of your own making. In addition, its informative type error reporting will be a good help to your writing.

This software was supported by:

* IPA Mitou Project 2017 (June 2017 – February 2018; see the abstract [here](https://www.ipa.go.jp/jinzai/mitou/2017/gaiyou_t-4.html) written in Japanese),
* Dwango Co., Ltd. (October 2018 – March 2019; as a part-time job), and
* many anonimous supporters who bought [The SATySFi​book](https://booth.pm/ja/items/1127224),

and its development continues to this day (January 2020).

## Install using Homebrew (for OS X users)

A Homebrew formula is provided for SATySFi (v0.0.2).

```sh
$ brew install --HEAD nyuichi/satysfi/satysfi
```

## Install using OPAM

### Prerequisites

Here is a list of minimally required softwares.

* bzip2
* cc
* git
* m4
* make
* unzip
* wget or curl
* ruby
* [opam](https://opam.ocaml.org/) 2.0 (Installation instructions are [here](https://opam.ocaml.org/doc/Install.html).)
    * Bubblewrap, a tool required for opam 2, cannot be installed easily yet on some kinds of environment such as Windows Subsystem for Linux (WSL) and Ubuntu 16.04. As a workaround for the time being, opam 2 can be installed without bubblewrap by passing `--disable-sandboxing` option when running `opam init`. **Please see [opam's FAQ](https://opam.ocaml.org/doc/FAQ.html#Why-does-opam-require-bwrap) for details.**
* ocaml 4.06.0 (installed by OPAM)

Also, we must add an external OPAM repo to build. This can be done by the following command.

```sh
opam repository add satysfi-external https://github.com/gfngfn/satysfi-external-repo.git
opam update
```

#### Example (Ubuntu)

```sh
sudo apt-get update
sudo apt-get install build-essential git m4 unzip curl

sh <(curl -sL https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh)

# The following command will ask if you allow OPAM to modify some files (e.g. ~/.bash_profile).
# Be sure to read its instructions. Otherwise, some environment variables won't be set.
opam init --comp 4.06.0

eval $(opam env)

opam repository add satysfi-external https://github.com/gfngfn/satysfi-external-repo.git
opam update
```

#### Example (OS X Mavericks or later)

```sh
# Before running this scripts, install essential softwares such as GCC and Make. They can be installed from Xcode Command Line Tools.
# Also, install Homebrew.

brew update
brew install opam

# The following command will ask if OPAM modifies some files.
# Be sure to read their instructions. Otherwise, some environment variables won't be set.
opam init --comp 4.06.0

eval $(opam env)

opam repository add satysfi-external https://github.com/gfngfn/satysfi-external-repo.git
opam update
```

### Build

First, clone this repository and submodules. Then build SATySFi using OPAM.

```sh
# clone
git clone https://github.com/gfngfn/SATySFi.git
cd SATySFi

# build
opam pin add satysfi .
opam install satysfi
```

* To reinstall, run `opam reinstall satysfi`.
* To uninstall, run `opam uninstall satysfi`.

<!--
### Manual build of SATySFi

1. Install ocamlbuild, ocamlfind, and Menhir.
2. In repository, run `make`.
3. `macrodown` should then be available under the diretory.
4. Run `make install` to install `satysfi` as `/usr/local/bin/satysfi`.
5. Run `make install-lib` to create a symbolic link for the library.

You can modify the directory for the installation by specifying `PREFIX` like `sudo make install PREFIX=/usr/bin`. the symbolic link for the SATySFi library will be created as `/usr/local/lib-satysfi -> DIR/lib-satysfi` where `DIR` is the top directory of the repository.
-->

<!--
### Download release from GitHub

See [release page](https://github.com/gfngfn/Macrodown/releases)
-->

### Setup for SATySFi

Before using SATySFi, one should put libraries and fonts onto the appropriate directory. This can be done by invoking the following commands in order:

```sh
./download-fonts.sh
./install-libs.sh
```

The former downloads the fonts required by the default settings into `lib-satysfi/dist/fonts/`, and the latter copies `lib-satysfi/` to  `/usr/local/share/satysfi/`.

During this setup, the following fonts are downloaded. Consult their license before using them.

* [Junicode](http://junicode.sourceforge.net)
* [IPA Font](https://ipafont.ipa.go.jp/old/ipafont/download.html)
* [Latin Modern](http://www.gust.org.pl/projects/e-foundry/latin-modern/)
* [Latin Modern Math](http://www.gust.org.pl/projects/e-foundry/lm-math)

## Usage of SATySFi

Type

```sh
satysfi <input file> -o <output file>
```

in order to convert `<input file>` into `<output file>`. For example, when you want to convert `doc.saty` into `output.pdf`, the following command will work:

```sh
satysfi doc.saty -o output.pdf
```

### Starting out

First of all, let’s try to compile the demo file. It is in `demo` folder. Because this demo file has `MakeFile`, All you should do is only type `make`.

```sh
cd demo
make
```

If `demo.pdf` is created, then the setup has been finished correctly.

### Reference

In addition, a concice reference of SATySFi is written by SATySFi itself in `doc` folder. You need to compile it to read.

```sh
cd doc
make
```

## Command-line options

* `-v`, `--version`: Prints the version.
* `-o`, `--output`: Specify the name of the output PDF file. if this option is not given explicitly, the name of the output file is the concatenation of the base name of the input file and the extension `.pdf`.
* `-b`, `--bytecomp`: Use byte compiler and enhance performance of computation.
* `--full-path`: Displays file names with their absolute path when outputting them to stdout.
* `--type-check-only`: Stops after type checking.
* `--debug-show-bbox`: Outputs bounding boxes for each glyph (for the purpose of debugging).
* `--debug-show-space`: Outputs boxes for spaces (for the purpose of debugging).

## Learning SATySFi

[Wiki](https://github.com/gfngfn/SATySFi/wiki/SATySFi-Wiki#%E5%AD%A6%E7%BF%92%E7%94%A8%E8%B3%87%E6%96%99) (currently written only in Japanese) has some information about learning SATySFi.
