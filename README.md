# stata-exec

Send R code from Atom to be executed in R.app, Terminal, iTerm, or a web browser running RStudio Server on Mac OS X.  The current selection is sent or in the case of no selection the current line is sent.

## Installation

`apm install stata-exec`

or

Search for `stata-exec` within package search in the Settings View.

## Configuration

### Keybindings

While `cmd-enter` is bound to sending code in the package, it is also annoyingly bound to entering a new line by default in atom.
In order to make it work, you must add the following binding in `~/.atom/keymap.cson`:

```javascript
'atom-workspace atom-text-editor:not([mini])':
  'cmd-enter': 'stata-exec:send-command'
```

### Behavior

All configuration can be done in the settings panel. Alternatively, you can edit your configuration file as noted below.

In your global configuration file (`~/.atom/init.coffee`), you may set the following variables:

- `stata-exec.whichApp` which R application to use. Valid applications are:
  - `R.app`: the default (the R GUI)
  - `iTerm` or `Terminal`: Assumes the currently active terminal has R running
  - `Safari` or `Google Chrome`: assumes the currently active tab has an active RStudio session running, with the console active
- `stata-exec.advancePosition`
  - if `true`, go to the next line/paragraph after running the current line/paragraph
  - if `false`, leave the cursor where it currently is
- `stata-exec.focusWindow`
  - if `true`, focus the window before sending code
  - if `false`, send the code in the background and stay focused on Atom. This is not possible when sending code to a browser
- `stata-exec.notifications`
  - if `true`, notifications via `NotificationManager` when a paragraph or function is not identified

The default configuration looks like this:

```javascript
atom.config.set('stata-exec.whichApp', 'R.app')
atom.config.set('stata-exec.advancePosition', false)
atom.config.set('stata-exec.focusWindow', true)
atom.config.set('stata-exec.notifications', true)
```

### Notes about iTerm

The iTerm2 Applescript API recently changed as of version 3.0.0.
Older versions of iTerm2 (< 3.0.0) are supported using mode `iTerm`.
Newer versions of iTerm2 (>= 3.0.0) are supported using mode `iTerm2`.

## Usage

- `cmd-enter`: send code to configured application (`stata-exec:whichApp`).
- `shift-cmd-e`: change to current working directory of current file.
- `shift-cmd-k`: send code between a knitr block (currently only RMarkdown supported).
- `shift-cmd-u`: send function under current cursor. Currently, only functions that begin of the first column in and on the first column of a line are sent. An example:
```r
myFunction <- function(x) {
  # my code goes here
}
```
- `shift-cmd-m`: send paragraph under current cursor. A paragraph is a region enclosed by whitespace.

## Notes

This is very much in an **alpha** state and is a quick hobby project.  It is currently Mac-only because these things are easy to do with AppleScript.  Any help on the Windows or Linux side would be great.

In the RStudio Server case, the solution is pretty clunky - the code is sent to the clipboard and then a paste command is sent to Safari.  But it works.

## TODO

- Make the choice of which R.app to send the code to configurable, based on a project-level configuration variable (sometimes multiple copies of R.app are made so that multiple R GUIs can be running simultaneously for different projects).
- In RStudio Server case, make sure active window really is RStudio Server before pasting, maybe by checking the  [title](http://www.alfredforum.com/topic/2013-how-to-get-frontmost-tab's-url-and-title-of-various-browsers/).
- Error reporting.
- Support for Windows and Linux.
