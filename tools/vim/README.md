# vim

[https://cheatography.com/ericg/cheat-sheets/vi-editor/](https://cheatography.com/ericg/cheat-sheets/vi-editor/)

## Modes <a href="#title_270_1164" id="title_270_1164"></a>

| Vi has two modes insertion mode and command mode. The editor begins in command mode, where the cursor movement and text deletion and pasting occur. Insertion mode begins upon entering an insertion or change command. \[ESC] returns the editor to command mode (where you can quit, for example by typing :q!). Most commands execute as soon as you type them except for "­col­on" commands which execute when you press the ruturn key. |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

## Quitting <a href="#title_270_1165" id="title_270_1165"></a>

| :x  | Exit, saving changes                        |
| --- | ------------------------------------------- |
| :q  | Exit as long as there have been no changes  |
| ZZ  | Exit and save changes if any have been made |
| :q! | Exit and ignore any changes                 |



## Inserting Text <a href="#title_270_1166" id="title_270_1166"></a>

| i | Insert before cursor                |
| - | ----------------------------------- |
| I | Insert before line                  |
| a | Append after cursor                 |
| A | Append after line                   |
| o | Open a new line after current line  |
| O | Open a new line before current line |
| r | Replace one character               |
| R | Replace many characters             |



## Motion <a href="#title_270_1163" id="title_270_1163"></a>

| h              | Move left                                    |
| -------------- | -------------------------------------------- |
| j              | Move down                                    |
| k              | Move up                                      |
| l              | Move right                                   |
| w              | Move to next word                            |
| W              | Move to next blank delimited word            |
| b              | Move to the beginning of the word            |
| B              | Move to the beginning of blank delimted word |
| e              | Move to the end of the word                  |
| E              | Move to the end of Blank delimited word      |
| (              | Move a sentence back                         |
| )              | Move a sentence forward                      |
| {              | Move a paragraph back                        |
| }              | Move a paragraph forward                     |
| 0              | Move to the begining of the line             |
| $              | Move to the end of the line                  |
| 1G             | Move to the first line of the file           |
| G              | Move to the last line of the file            |
| _&#x6E;_&#x47; | Move to _&#x6E;_&#x74;h line of the file     |
| :_n_           | Move to _&#x6E;_&#x74;h line of the file     |
| &#x66;_&#x63;_ | Move forward to _c_                          |
| &#x46;_&#x63;_ | Move back to _c_                             |
| H              | Move to top of screen                        |
| M              | Move to middle of screen                     |
| L              | Move to botton of screen                     |
| %              | Move to associated ( ), { }, \[ ]            |
| :0             | Move to the beginning of the file            |
| :$             | Move to the end of the file                  |
| \[ctrl] + d    | go down half a screen                        |
| \[ctrl] + u    | go up half a screen                          |
| \[ctrl] + f    | go forward a screen                          |
| \[ctrl] + b    | go back a screen                             |

## Deleting Text <a href="#title_270_1167" id="title_270_1167"></a>

| x  | Delete character to the right of cursor |
| -- | --------------------------------------- |
| X  | Delete character to the left of cursor  |
| D  | Delete to the end of the line           |
| dd | Delete current line                     |
| :d | Delete current line                     |

## Yanking Text <a href="#title_270_1168" id="title_270_1168"></a>

| yy | Yank the current line |
| -- | --------------------- |
| :y | Yank the current line |



## Changing text <a href="#title_270_1169" id="title_270_1169"></a>

| C   | Change to the end of the line |
| --- | ----------------------------- |
| cc  | Change the whole line         |
| guu | lowercase line                |
| gUU | uppercase line                |
| \~  | Toggle upp and lower case     |

## Putting text <a href="#title_270_1170" id="title_270_1170"></a>

| p | Put after the position or after the line  |
| - | ----------------------------------------- |
| P | Put before the poition or before the line |

## Markers <a href="#title_270_1171" id="title_270_1171"></a>

| &#x6D;_&#x63;_ | Set marker _c_ on this line                         |
| -------------- | --------------------------------------------------- |
| \`_c_          | Go to beginning of marker _c_ line.                 |
| '_c_           | Go to first non-blank character of marker _c_ line. |

## Search for strings <a href="#title_270_1172" id="title_270_1172"></a>

| /string | Search forward for string              |
| ------- | -------------------------------------- |
| ?string | Search back for string                 |
| n       | Search for next instance of string     |
| N       | Search for previous instance of string |

## Replace <a href="#title_270_1173" id="title_270_1173"></a>

| :s/pat­ter­n/s­tri­ng/­flags | Replace pattern with string according to flags. |
| ---------------------------- | ----------------------------------------------- |
| g                            | Flag - Replace all occurences of pattern        |
| c                            | Flag - Confirm replaces.                        |
| &                            | Repeat last :s command                          |

## Ranges <a href="#title_270_1174" id="title_270_1174"></a>

| :_n_,_m_     | Range - Lines _n_-_m_                  |
| ------------ | -------------------------------------- |
| :.           | Range - Current line                   |
| :$           | Range - Last line                      |
| :'_c_        | Range - Marker _c_                     |
| :%           | Range - All lines in file              |
| :g/pat­tern/ | Range - All lines that contain pattern |

## Files <a href="#title_270_1175" id="title_270_1175"></a>

| :w _file_   | Write to _file_                         |
| ----------- | --------------------------------------- |
| :r _file_   | Read _file_ in after line               |
| :n          | Go to next file                         |
| :p          | Go to previous file                     |
| :e _file_   | Edit _file_                             |
| !!_program_ | Replace line with output from _program_ |

## Other <a href="#title_270_1176" id="title_270_1176"></a>

| J | Join lines                         |
| - | ---------------------------------- |
| . | Repeat last text-c­hanging command |
| u | Undo last change                   |
| U | Undo all changes to line           |
