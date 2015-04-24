# Introduction #

Bash is really powerfull. There is a lot of commands to make everything you need. Bash simple curses uses
  * tput command
  * STD redirection
  * escaped colors

Let me explain how I did


## Lines ##

Lines and corners are display as "chars". You can try this:
```
echo -e "\033(0 l q k x m j \033(B"
```

You will see special chars used to create windows.

## Placing cursor ##

Because we need to write lines and texts on screen, "tput" command is used. tput can move cursor everywhere you want.

## Colors ##

Bash can change the text color using escaped values. For example
```
echo -e "\033[32mText in red\033[0m"
```

This display text in red color.
## Buffer ##

Tput command is a bit low... Refreshing view is not pretty while the cursors is moving on screen. A "clipping" appears. That's why Bash simple curses needs a STDOUT buffer that is not display until we explicitally ask to flush display.

But... Bash has no STDOUT buffer...

**But Bash is powerfull I said !** If you change colors, write texts, and you redirect to a file, Bash insert special caracters to set colors while you use "cat" command.

Bash simple curses execute every "echo" command redirected to /tmp/bashbar.buffer

This buffer is flushed when "refresh" command is called.

## What's append ? ##

Everything is done when you have call "main\_loop" function. This makes:
  * clean scree
  * place cursor on top
  * initiate buffer

When you create a "window", a title is set with color and size. Size is kept to set content with same width.

Everytime you call "window" or "append", a basic method is called to place text on center.
Then "endwin" close window.

"main\_loop" send outuput to the buffer file, when everything on "main" function is done, "main\_loop" display buffer then clean it.