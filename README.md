# What is Lime?

I love the [Sublime Text](http://www.sublimetext.com) editor. [I have](https://github.com/quarnster/SublimeClang) [created](https://github.com/quarnster/SublimeJava) [several](https://github.com/quarnster/CompleteSharp) [plugins](https://github.com/quarnster/SublimeGDB) [to make](https://github.com/quarnster/ADBView) it even better. One thing that scares me though is that it is not open sourced and the [pace of nightly releases](http://www.sublimetext.com/nightly) have recently been anything but nightly, even now that version 3 is out in Beta.

There was a period of about 6 months after the Sublime Text 2 “stable“ version was released where pretty much nothing at all was communicated to the users about what to expect in the future, nor was there much support offered in the forums. People including myself were wondering if the product was dead and I personally wondered what would happen to all the bugs, crashes and annoyances that still existed in ST2. This lack of communication is a dealbreaker to me and I decided that I will not spend any more money on that product because of it.

As none of the other text editors I've tried come close to the love I had for Sublime Text, I decided I had to create my own.

The frontend(s) are not ready to replace your favourite editor, but the backend itself I believe isn't too far away.

![Screenshot taken Oct 23 2013](http://i.imgur.com/VIpmjau.png)

# Goals

- ☑ 100% Open source
- ☑ Compatible with Textmate color schemes (which is what ST is using)
- ☑ Compatible with Textmate syntax definitions (which again is what ST is using)
- ☐ Compatible with Textmate snippets
- ☐ Compatible with Sublime Text’s python plugin API. I’ll probably never implement this 100%, only the api bits I need for the plugins I use.
- ☐ Compatible with Sublime Text’s keybindings and settings
- ☐ Compatible with Sublime Text snippets
- ☐ Sublime Text’s Goto anything panel
- ☑ Multiple cursors
- ☑ Regression tests (Programming in [Go](http://golang.org) makes it trivial and even fun to write them ;))
- ☐ Support for plugging in a custom parser for more advanced syntax highlighting.
- ☐ Terminal UI (*Maybe* I’ll work on a simple non-terminal UI at some point)
- ☐ Cross platform (It appears to be compiling and running on OSX and Linux last I tried, but needs further validation.)

# Why can’t I open up an issue?

Because I’m just a single person and I don’t want to offer up my spare time doing support or dealing with feature requests that I don’t care about myself. If you want a feature implemented or a bug fixed, fork it and implement it yourself and submit a pull request when you’re happy with the implementation.

# Build instructions

### Install required components
- Go 1.1
   - Follow the build instructions at [tip.golang.org](http://tip.golang.org/doc/install/source)
- Python3
   - Python 3 **must** be compiled without [sigaltstack enabled](https://code.google.com/p/go/issues/detail?id=5287).
   - ~~``` sudo apt-get install python3-dev ``` # On Linux~~
   - ~~``` brew install python3 ``` # On Mac~~
- Oniguruma
   - ``` sudo apt-get install libonig-dev ``` # On Linux
   - ``` brew install oniguruma ``` # On Mac
- qt5 (Optional)
   - Follow the instructions at [go-qt5](https://github.com/salviati/go-qt5)

### Download the needed repositories

```
go get code.google.com/p/log4go github.com/quarnster/parser github.com/quarnster/completion github.com/howeyc/fsnotify
git clone --recursive git@github.com:quarnster/lime.git $GOPATH/src
```

### Modify cgo.go settings

``` open $GOPATH/src/lime/3rdparty/libs/gopy/lib/cgo.go ```

Example of ``` cgo.go ``` settings on my Mac

```
package py

// #cgo CFLAGS: -I/usr/local/Cellar/python3/3.3.1/Frameworks/Python.framework/Versions/Current/include/python3.3m -I/usr/local/Cellar/python3/3.3.1/Frameworks/Python.framework/Versions/Current/include/python3.3m
// #cgo LDFLAGS: -L/usr/local/Cellar/python3/3.3.1/Frameworks/Python.framework/Versions/Current/lib/python3.3/config-3.3m -ldl -framework CoreFoundation -lpython3.3
// #cgo pkg-config: /usr/local/Cellar/libffi/3.0.13/lib/pkgconfig/libffi.pc
import "C"
```

### Compile completion

```
cd $GOPATH/src/github.com/quarnster/completion/build
make
```

### Compile lime

```
cd $GOPATH/src/lime/build
go run build.go
```

Done!

# To use termbox frontend

```
cd ../frontend/termbox
go run main.go
```

# To use qt5 frontend

```
cd ../frontend/qt5
go run main.go
```

# License

The license of the project is the 2-clause BSD license:

```
Copyright (c) 2013 Fredrik Ehnbom
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

1. Redistributions of source code must retain the above copyright notice, this
   list of conditions and the following disclaimer.
2. Redistributions in binary form must reproduce the above copyright notice,
   this list of conditions and the following disclaimer in the documentation
   and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR
ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
```
