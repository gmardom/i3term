This script makes managing terminals under [i3wm] easier.
It uses various scripts from [i3ass] and works with the
following terminals:  

- **URxvt**
- **XTerm**
- **xfce4-terminal**
- **st**
- **kitty**
## installation
```
$ git clone https://github.com/budlabs/i3term.git
$ cd i3term
$ make
# make install
```

#### build dependencies
[GNU make], [Gawk], [bash], [GNU sed](https://www.gnu.org/software/sed/)  

#### runtime dependencies
- [i3ass]  
- [rofi] (*to use*: `i3term --palette-menu`)  
- `xterm|urxvt|xfce4-terminal|st|kitty`
## usage
`i3term` needs to know which terminal emulator to
use. Make sure it is declared in
the **configuration file** (`~/.config/i3term/config`).
```
i3term [OPTIONS]
-a, --autotile                            | always creates a new window with instance "auto"
--bg                         COLOR        | set background color
-d, --cd                     DIR          | set initial working directory 
--dont-beam-me-up-scotty                  | no beam cursor
--dryrun                                  | dont execute commands  
--fg                         COLOR        | set foreground color
-f, --font                   FONT         | set font 
--font-size                  INT          | set font-size 
-G, --geometry               GEOMETRY     | set gemoetry (COLUMNSxLINES)
-h, --help                                | display help and exit 
--hide                                    | option will get redirected to i3run 
-i, --instance               INSTANCE     | instance name to target
--kitty-options              OPTIONS      | extra options passed to kitty
--large-font                              | if setting "large_font" is set it will be used 
-l, --list-palettes                       | lists available palettes
--login                                   | passes '-l' to the shell 
-m, --mouse                               | option will get redirected to i3run 
--no-exec                                 | short description  
-g, --nohide                              | option will get redirected to i3run 
-p, --palette                PALETTE|FILE | use the colors from palette
--palette-menu                            | short description  
--role                       ROLE         | this have the exact same function as --instance
--shell                      SHELL        | set shell, defaults to `$SHELL`
--st-options                 OPTIONS      | extra options passed to st
-s, --summon                              | option will get redirected to i3run 
--terminal                   TERMINAL     | terminal emulator (urxvt|xterm|st|xfce4-terminal|kitty)
--urxvt-options              OPTIONS      | extra options passed to urxvt
-V, --verbose                             | print command and script file to stderr  
-v, --version                             | print version and exit 
--xfce4-terminal-options     OPTIONS      | extra options passed to xfce4-terminal
--xterm-options              OPTIONS      | extra options passed to xterm
```
## examples
### i3 keybinding configuration

The `$exec` and `$super` variables are compatible 
with the configuration example from the [i3ass wiki].

```
set $exec exec --no-startup-id exec
set $super bindsym Mod4

$super+Return         $exec i3term --instance mainterm --large-font --palette base16-grayscale-light
$super+Control+Return $exec i3term --autotile
$super+Shift+Return   $exec i3term --instance floatterm --summon --geometry 50x8
$super+t              $exec i3term --palette-menu
$super+v              $exec typisktstart
```
### i3king config examples

```text
# TC is a variable for i3fyra --move A|B|C|D
set $TC exec --no-startup-id i3fyra --conid $CONID --move
set $X  exec --no-startup-id

# ignore terminals with instance name "auto" (--autotile)
# from the DEFAULT rule.
DEFAULT \
  class=(URxvt|XTerm|st-256color|kitty) instance=auto, \
  class=Xfce4-terminal role=auto
    floating enable, border normal 2, title_window_icon padding 3px

# instace match XTerm|URxvt|st|kitty , role matches Xfce4-terminal
instance=mainterm, role=mainterm
  $TC A

# floatterm is put in a specific location with i3Kornhe
instance=floatterm, role=floatterm
  floating enable;                          \
  $X i3Kornhe --oneshot --margin 40 move 2  \

# typiskt is put on workspace 2
instance=typiskt, role=typiskt
  move to workspace 2, floating enable;        \
    $X i3Kornhe --oneshot --margin 300 move 4; \
    workspace 2

instance=sidplayfp class=(URxvt|XTerm|st-256color|kitty), \
role=sidplayfp class=Xfce4-terminal
  $TC A
```
### sidopen

This script can be used to open *.sid* files (C64 music files).
With [sidplayfp] in a terminal using the C64 font and colors.
![sidplay](https://user-images.githubusercontent.com/2143465/180494442-8d05be72-7ece-4b72-ba9d-02096316c622.png)

```bash
#!/bin/bash

# aur/ttf-c64
# extra/libsidplayfp
i3term --instance sidplayfp          \
       --bg '#2E2C9B' --fg '#706DEB' \
       --font ' C64 Pro Mono'        \
       --geometry 56x16              \
  -- sidplayfp -q "$1"
```
### typisktstart

This is just a wrapper script around [typiskt]. It is
convenient to change properties in its own script.
![typiskt](https://user-images.githubusercontent.com/2143465/180494963-6053eddb-6bc8-4493-a75f-e64179344df0.png)

```bash
#!/bin/bash

# aur/ttf-go-mono-git
# aur/typiskt
i3term --font "Go Mono" --font-size "16" \
       --palette clrs-light              \
       --geometry 50x12                  \
  -- typiskt --corpus english --lines 3
```
[i3wm]: https://i3wm.org
[i3ass]: https://github.com/budlabs/i3ass
[i3ass wiki]: https://github.com/budlabs/i3ass/wiki
[typiskt]: https://github.com/budlabs/typiskt
[sidplayfp]: https://github.com/libsidplayfp/libsidplayfp

[youtube channel]: https://youtube.com/c/dubbeltumme
[rofi]: https://github.com/davatorium/rofi
[Gawk]: https://www.gnu.org/software/gawk/
[bash]: https://www.gnu.org/software/bash/
[lowdown]: https://kristaps.bsd.lv/lowdown/
[GNU make]: https://www.gnu.org/software/make/
[xdotool]: https://www.semicomplete.com/projects/xdotool/
