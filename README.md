# readline

[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE.md)
[![Build Status](https://travis-ci.org/chzyer/readline.svg?branch=master)](https://travis-ci.org/chzyer/readline)
[![GoDoc](https://godoc.org/github.com/chzyer/readline?status.svg)](https://godoc.org/github.com/chzyer/readline)  

Readline is A Pure Go Implementation of a libreadline-style Library.  
The goal is to be a powerful alternater for GNU-Readline.


**WHY:**
Readline will support most of features which GNU Readline is supported, and provide a pure go environment and a MIT license.

# Demo

![demo](https://raw.githubusercontent.com/chzyer/readline/master/example/demo.gif)

You can read the source code in [example/main.go](https://github.com/chzyer/readline/blob/master/example/main.go).

# Todo

* Vim mode
* More funny examples

# Usage

* Simplest example

```go
import "github.com/chzyer/readline"

rl, err := readline.New("> ")
if err != nil {
	panic(err)
}
defer rl.Close()

for {
	line, err := rl.Readline()
	if err != nil { // io.EOF
		break
	}
	println(line)
}
```

* Example with durable history

```go
rl, err := readline.NewEx(&readline.Config{
	Prompt: "> ",
	HistoryFile: "/tmp/readline.tmp",
})
if err != nil {
	panic(err)
}
defer rl.Close()

for {
	line, err := rl.Readline()
	if err != nil { // io.EOF
		break
	}
	println(line)
}
```

* Example with auto refresh

```go
import (
	"log"
	"github.com/chzyer/readline"
)

rl, err := readline.New("> ")
if err != nil {
	panic(err)
}
defer rl.Close()
log.SetOutput(l.Stderr()) // let "log" write to l.Stderr instead of os.Stderr

go func() {
	for _ = range time.Tick(time.Second) {
		log.Println("hello")
	}
}()

for {
	line, err := rl.Readline()
	if err != nil { // io.EOF
		break
	}
	println(line)
}
```

* Example with auto completion

```go
import (
	"log"
	"github.com/chzyer/readline"
)

var completer = readline.NewPrefixCompleter(
	readline.PcItem("say",
		readline.PcItem("hello"),
		readline.PcItem("bye"),
	),
	readline.PcItem("help"),
)

rl, err := readline.New(&readline.Config{
	Prompt:       "> ",
	AutoComplete: completer,
})
if err != nil {
	panic(err)
}
defer rl.Close()

for {
	line, err := rl.Readline()
	if err != nil { // io.EOF
		break
	}
	println(line)
}
```


# Shortcut

`Meta`+`B` means press `Esc` and `n` separately.  
Users can change that in terminal simulator(i.e. iTerm2) to `Alt`+`B`

* Shortcut in normal mode

| Shortcut           | Comment                                  |
|--------------------|------------------------------------------|
| `Ctrl`+`A`         | Beginning of line                        |
| `Ctrl`+`B` / `←`   | Backward one character                   |
| `Meta`+`B`         | Backward one word                        |
| `Ctrl`+`C`         | Send io.EOF                              |
| `Ctrl`+`D`         | Delete one character                     |
| `Meta`+`D`         | Delete one word                          |
| `Ctrl`+`E`         | End of line                              |
| `Ctrl`+`F` / `→`   | Forward one character                    |
| `Meta`+`F`         | Forward one word                         |
| `Ctrl`+`G`         | Cancel                                   |
| `Ctrl`+`H`         | Delete previous character                |
| `Ctrl`+`I` / `Tab` | Command line completion                  |
| `Ctrl`+`J`         | Line feed                                |
| `Ctrl`+`K`         | Cut text to the end of line              |
| `Ctrl`+`L`         | Clean screen (TODO)                      |
| `Ctrl`+`M`         | Same as Enter key                        |
| `Ctrl`+`N` / `↓`   | Next line (in history)                   |
| `Ctrl`+`P` / `↑`   | Prev line (in history)                   |
| `Ctrl`+`R`         | Search backwards in history              |
| `Ctrl`+`S`         | Search forwards in history               |
| `Ctrl`+`T`         | Transpose characters                     |
| `Meta`+`T`         | Transpose words (TODO)                   |
| `Ctrl`+`U`         | Cut text to the beginning of line (TODO) |
| `Ctrl`+`W`         | Cut previous word                        |
| `Backspace`        | Delete previous character                |
| `Meta`+`Backspace` | Cut previous word                        |
| `Enter`            | Line feed                                |


* Shortcut in Search Mode (`Ctrl`+`S` or `Ctrl`+`r` to enter this mode)

| Shortcut                | Comment                                     |
|-------------------------|---------------------------------------------|
| `Ctrl`+`S`              | Search forwards in history                  |
| `Ctrl`+`R`              | Search backwards in history                 |
| `Ctrl`+`C` / `Ctrl`+`G` | Exit Search Mode and revert the history     |
| `Backspace`             | Delete previous character                   |
| Other                   | Exit Search Mode                            |

* Shortcut in Complete Select Mode (double `Tab` to enter this mode)

| Shortcut                | Comment                                     |
|-------------------------|---------------------------------------------|
| `Ctrl`+`F`              | Move Forward                                |
| `Ctrl`+`B`              | Move Backward                               |
| `Ctrl`+`N`              | Move to next line                           |
| `Ctrl`+`P`              | Move to previous line                       |
| `Ctrl`+`A`              | Move to the first candicate in current line |
| `Ctrl`+`E`              | Move to the last candicate in current line  |
| `Tab` / `Enter`         | Use the word on cursor to complete          |
| `Ctrl`+`C` / `Ctrl`+`G` | Exit Complete Select Mode                   |
| Other                   | Exit Complete Select Mode                   |

# Tested with

| Environment                   | $TERM  |
|-------------------------------|--------|
| Mac OS X iTerm2               | xterm  |
| Mac OS X default Terminal.app | xterm  |
| Mac OS X iTerm2 Screen        | screen |
| Mac OS X iTerm2 Tmux          | screen |
| Ubuntu Server 14.04 LTS       | linux  |
| Centos 7                      | linux  |

### Notice:
* `Ctrl`+`A` is not working in screen because it used as a control command by default

If you test it otherwhere, whether it works fine or not, please let me know!

# Feedback

If you have any question, please submit an GitHub Issues and any pull request is welcomed :)
