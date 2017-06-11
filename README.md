# mate-hud

Provides a way to run menubar commands through
[rofi](https://davedavenport.github.io/rofi/), much like the Unity 7
Heads-Up Display (HUD). `mate-hud` was originally forked from
`i3-hud-menu`:

  * https://jamcnaughton.com/2015/10/19/hud-for-xubuntu/
  * https://github.com/jamcnaughton/i3-hud-menu
  * https://github.com/RafaelBocquet/i3-hud-menu

It was subsequently improved by incorporating improvements from
[snippins](https://gist.github.com/snippins)

  * [HUD.py](https://gist.github.com/snippins/ee943f2b25db555ef12107f7cee20241)

## What is a HUD and why should I care?

A Heads-Up Display (HUD) allows you to search through an application's
appmenu. So if you're trying to find that single filter in Gimp but
can't remember which filter category it fits into or if you can't
recall if preferences sits under File, Edit or Tools on your favourite
browser, you can just search for it rather than hunting through the
menus.

### Implementation

[vala-panel-appmenu](https://github.com/rilian-la-te/vala-panel-appmenu)
includes an implementation of the `com.canonical.AppMenu.Registrar` DBus
service. Applications exporting their menu via `dbusmenu` need this
service to run. `mate-hud.py` tries to get the menu of the currently
focused window, lists possible actions and asks the user which one to
run. `mate-hud.py`, binds itself to the `<Ctrl><Alt>space` keyboard
shortcut by default.

### Gsettings

`mate-hud.py` reads one gsettings keys:

  * `org.mate.hud`: `shortcut` (Default: <Ctrl><Alt>space)

`mate-hud.py` will not execute until those gsettings keys are created,
which the `mate-hud` Debian package will do, and the `enabled` key
is set to *True* using something like `dconf-editor`. MATE Tweak
will soon add the functionality the endable/disable `mate-hud`.

### Manual Setup

  * The `vala-panel-appmenu` applet for MATE should be added to a panel.
  * `mate-hud.py` should be started on session start-up.
  * The following should be added to the users `~/.profile` or `/etc/profile.d` or `/etc/X11/Xsession.d/`.

```
if [ -n "$GTK_MODULES" ]; then
    GTK_MODULES="$GTK_MODULES:unity-gtk-module"
else
    GTK_MODULES="unity-gtk-module"
fi

export GTK_MODULES
export UBUNTU_MENUPROXY=1
```

## Dependencies

  * `appmenu-qt`
  * `gir1.2-keybinder-3.0`
  * `gir1.2-gtk-3.0`
  * `mate-desktop`
  * `python3`
  * `python3-dbus`
  * `python3-setproctitle`
  * `python3-xlib`
  * `rofi`
  * `unity-gtk2-module`
  * `unity-gtk3-module`

A reference package for Debian/Ubuntu is available from:

  * https://bitbucket.org/flexiondotorg/debian-packages