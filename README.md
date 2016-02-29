#!/bin/bash -euo pipefail #<!--
export MONOFILE="$0"; cat "$0" | awk '/\<a name\=\"2\.1\.\"\>/{y=1;next}y' | tail -n+3 | bash -euo pipefail -- /dev/stdin "$@"; exit 0
-->

# Monofile

What would be the perfect way to reduce all of your problems down to one single
problem? Putting *everything* in the same file 🙌

## Table of contents

1. [Monofile documentation](#1.)
2. [Monofile implementation](#2.)
3. [Your code 😎](#3.)

-----

# 1. Monofile documentation <a name="1."></a>

Monofile couldn't be easier to use, everything you need is in this file. For
convenience we have published this file to [npm](https://npmjs.com/), but you
could just as easy download this file and put it in your path.

## 1.1. Installation

```bash
npm install --global monofile
```

## 1.2. Usage

To initialise a new monofile, run the following command to create a new
`README.md` file in the current directory.

```bash
monofile init
```

To run your command, use the builtin `run` command.

```bash
./README.md run
```

# 2. Monofile implementation <a name="2."></a>

## 2.1. Bootstrapping  <a name="2.1."></a>

```bash
if [ "$#" -ne 1 ]; then
  >&2 echo "Please give monofile exactly one argument"
  exit 1
fi

if [ "$1" == "init" ]; then
  if [ -e README.md ]; then
    >&2 echo "Monofile allready initialised"
    exit 1
  fi

  cat "$MONOFILE" > README.md
  chmod +x README.md
fi

if [ "$1" == "run" ]; then
  BOOTSTRAP_LANGUAGE="$(cat "$MONOFILE" | awk '/\<a name\=\"3\.1\.\"\>/{y=1;next}y' | tail -n+2 | head -n 1)"

  if [ "$BOOTSTRAP_LANGUAGE" == '```javascript' ]; then
    cat "$MONOFILE" | awk '/\<a name\=\"3\.1\.\"\>/{y=1;next}y' | tail -n+3 | awk '/```/{exit};1' | node
  else
    >&2 echo "Unsupported language: \"$BOOTSTRAP_LANGUAGE\""
    >&2 echo "Please file an issue at https://github.com/LinusU/monofile/issues"
    exit 1
  fi
fi

exit 0
```

# 3. Your code 😎 <a name="3."></a>

# 3.1. Bootstrapping <a name="3.1."></a>

```javascript
console.log('🔥  This is the place where your code should live 🔥')
```
