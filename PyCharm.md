# PyCharm hints

## Enable ide.suppress.double.click.handler key in Registry:

* Open Find Action... ("Ctrl-Shift-A" on Windows and Linux, "Cmd-Shift-A" on macOS)
* Type "Registry...", press Enter
* Find the key **ide.suppress.double.click.handler** (just start typing for speed searching) and enable it

## External file changes sync may be slow

* Add **-Didea.filewatcher.disabled=true** in Help | Edit Custom VM Options and restart the IDE.
