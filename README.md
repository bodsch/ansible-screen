
# Ansible Role:  `screen`

Ansible role to install and configure `screen`


[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/bodsch/ansible-screen/main.yml?branch=main)][ci]
[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/bodsch/ansible-screen/CI)][ci]
[![GitHub issues](https://img.shields.io/github/issues/bodsch/ansible-screen)][issues]
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/bodsch/ansible-screen)][releases]
[![Ansible Quality Score](https://img.shields.io/ansible/quality/50067?label=role%20quality)][quality]

[ci]: https://github.com/bodsch/ansible-screen/actions
[issues]: https://github.com/bodsch/ansible-screen/issues?q=is%3Aopen+is%3Aissue
[releases]: https://github.com/bodsch/ansible-screen/releases
[quality]: https://galaxy.ansible.com/bodsch/screen


## Requirements & Dependencies

None

### Operating systems

Tested on

* ArchLinux
* ArtixLinux
* Debian based
    - Debian 10 / 11
    - Ubuntu 20.04
* RedHat based
    - Alma Linux 8
    - Rocky Linux 8
    - OracleLinux 8

## usage


```yaml
screen_conf_file: "/etc/screenrc"
screen_autodetach: true

screen_bind: []

screen_bindkey: []

screen_caption: true
screen_caption_line: 'always "%{= 4m}%{+b w}[Screen: %n][ %H ] %= [ %d.%m.%y %c:%s ]"'

screen_hardstatus: false
screen_hardstatus_line: 'alwayslastline "%-Lw%{= rW} %50>%2n%f * %t %{-}%+Lw%<  %= "'

screen_screens: []

screen_defscrollback: 10240
screen_defutf8: true

screen_startup_message: false
screen_utf8: true
screen_verbose: false
screen_wrap: true
screen_zombie_timeout: 5
```

### `screen_bind`

```yaml
screen_bind:
  - key: '='
    command: resize =
  - key: '+'
    command: resize +1
  - key: '-'
    command: resize -1
  - key: '_'
    command: resize max
```

### `screen_bindkey`

```yaml
screen_bindkey:
  - key: "k1"
    description: binds F1  to select screen 1
    command: select 1
  - key: "k2"
    description: binds F2  to select screen 2
    command: select 2
```


### `screen_screens`

```yaml
screen_screens:
  - options: ""
    number: 1
    command: bash
  - options: "-t htop"
    number: 2
    command: htop
```

## Description


### `hardstatus`

[escape-strings](https://www.gnu.org/software/screen/manual/screen.html#String-Escapes)

| codes            | erklärung |
| :----            | :---- |
|`%{}`             | sind farbcommandos |
| `%H`             | ist der Hostname |
| `%? %?`          | Eine Art if |
| <code>%1`</code> | Commando 1 ausfuehren: [backtick](https://www.gnu.org/software/screen/manual/screen.html#Backtick) |
| `%=`             | padding (auffuellen mit nichts) |
| `=%`             | padding (auffuellen mit nichts) |
| `%-w %+w`        | Fenster vor bzw nach dem aktuellen Fenster auflisten |
| `%n`             | Fensternummer |
| `%t`             | Fenstertitel |
| `%d.%m.%y %c:%s` | Datum und Uhrzeit |

### `caption`

ist das gleiche wie hardstatus, aber fuer die unterregionen wenn man die konsole splittet.

### Anfangs geoeffnete Fenster

`screen -t <fenstername> <fensternummer> <programm>`

z.B.

`screen -t cam1  1 bash`

### Tastenkombinationen

`bind` erstellt eine Tastenkombination fuer *<Strg+A __>*

`bindkey` erstellt eine Tastenkombination die auch ohne *Strg+A* funktioniert.


#### Neue Tastenkombination erstellen

##### `bind`

`bind: <Taste> <Befehl ...>`

normalerweise ist der standard fuer `<Strg+A c>`: `bind c screen 0` dann wird - von 0 ausgehend - die erste freie nummer gesucht.

Die Nr 0 ist aber unpraktisch beim Screenwechseln `<Strg+A 0>`

##### `bindkey`

Die kryptischen Werte fuer bindkey erhaelt man, wenn man in der konsole `read` ausfuehrt und dann die tastenkombination tippt.

Bei gespitteten Fenstern mit <Strg+Rechts> usw navigieren:

```bash
bindkey "^[[1;5D" focus left
bindkey "^[[1;5C" focus right
bindkey "^[[1;5A" focus up
bindkey "^[[1;5B" focus down
```

Layouts wechseln mit <Strg+F3> (prev layout) und <Strg+F4> (next)

```bash
bindkey "^[[1;5R" layout prev
bindkey "^[[1;5S" layout next
```



### Layout Voreinstellungen

Ein Layout ist eine Fensteranordnung innerhalb von screen. 
Man kann naemlich die den Bereich splitten und mehrere Konsolen nebeneinander anzeigen.

Die folgenden Befehle erstellen neue layouts, splitten, fokussieren einen anderen
splitbereich, splitten nochmal usw. Die Reihenfolge machts hier.

Es kann parallel mehrere layouts geben, spaeter werden noch `<Strg+F3>` und `<Strg+F4>` definiert um zwischen den Layouts zu wechseln.

```bash
layout autosave on
layout new one
select 1
layout new two
select 1
split
resize -v +8
focus down
select 4
focus up

.
.
.

layout attach one
layout select one
```


---

## Author and License

- Bodo Schulz

## License

[Apache](LICENSE)

`FREE SOFTWARE, HELL YEAH!`
