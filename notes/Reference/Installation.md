## Install with a package manager

Follow the instructions for [installing NPM](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) relative to your system

The simplest and easiest way to install Madlib is using the [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) package manager.

```sh
npm install @madlib-lang/madlib -g
```

## Install by building locally

A slightly more complicated way of installing Madlib is to pull down the code repository from [Github](https://github.com/madlib-lang/madlib). You'll need to install [GHCup](https://www.haskell.org/ghc/) as well.

These instructions _should_ be up to date, but in the off chance that they aren't, we're going through the extant build process in the Madlib repo, available [here](https://github.com/madlib-lang/madlib/blob/master/.github/workflows/build.yml). Since that happens on an ubuntu virtual machine, these instructions cater to that box as well.

### Clone the repository

```sh
git clone https://github.com/madlib-lang/madlib
cd madlib
```

### Install GHCup

```sh
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
```

### Install the Haskell dependencies

```sh
ghcup install 8.10.7
ghcup install cabal 3.4.0.0
ghcup install stack 2.7.3
ghcup set 8.10.7
ghcup set cabal 3.4.0.0
ghcup set stack 2.7.3
```

### Configure stack

```sh
stack config set system-ghc --global true
```

### Install additional tools

#### Install additional tools on Linux

```sh
sudo apt-get install -y lcov llvm-12 nasm
```

#### Install additional tools on OSX using `homebrew`

Install [homebrew](https://brew.sh/) and then run:

```sh
brew install lcov llvm@12 nasm
```

### Install the lexing and parsing libraries

```sh
stack install alex happy
```

### Build the runtime libraries

Make sure your current working directory is the `madlib` repo, and then run this command:

```sh
TARGET="LINUX_X64" ./scripts/build-runtime-libs
```

### Build the runtime

```sh
AR="llvm-ar-12" TARGET="LINUX_X64" CPLUS_INCLUDE_PATH="/usr/include/c++/9:/usr/include/x86_64-linux-gnu/c++/9:/usr/include/" ./scripts/build-runtime
```