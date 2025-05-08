[github](https://github.com/v3xlabs/mission-control)

# Information Displays

Lets say you wanted to have a display in your office that is on all the time (24/7).
It toggles between maybe grafana dashboards, your favourite webistes, or your own custom content.

## From Scratch

### The Setup

For hardware you will want a screen (any old TV will do), and a small computer (like a Raspberry Pi or a Dell Optiplex).

### Software

Pre-requisites (based on Arch Linux):

```
chromium
xorg-server
xorg-xinit
openbox
xorg-xrandr
ddcutil
i2c-tools
konsole
nodejs
npm
```

Create `/etc/systemd/system/getty@tty1.service.d/override.conf` with the following content:
(replace `DISPLAY_USERNAME_HERE` with the username of the user you want to autologin as)

```ini
[Service]
ExecStart=
ExecStart=-/sbin/agetty --autologin DISPLAY_USERNAME_HERE --noclear %I $TERM
```

```sh
chmod 0644 /etc/systemd/system/getty@tty1.service.d/override.conf
```

Then create `/home/DISPLAY_USERNAME_HERE/.xinitrc` with the following content:

```sh
#!/bin/sh
# Dynamically detect the connected output and rotate it
OUTPUT=$(xrandr --query | grep " connected" | cut -d ' ' -f1 | head -n 1)
xrandr --output "$OUTPUT" --rotate right --brightness 1
exec openbox-session &
sleep 2

# Disable screen blanking and power management
xset s off
xset s noblank
xset -dpms

bun run /home/DISPLAY_USERNAME_HERE/script.js &
chromium --kiosk --noerrdialogs --disable-infobars --disable-session-crashed-bubble --disable-features=TranslateUI "CHROMIUM_URL_HERE"
```

Ensure its owned by the user and has the correct permissions: `0755`

Create `/home/DISPLAY_USERNAME_HERE/.bash_profile` (`0644` owned by user) with the following content:

```sh
#!/bin/sh
if [[ -z $DISPLAY ]] && [[ $(tty) == /dev/tty1 ]]; then
    startx
fi
```

## Mission Control

Lets say you have a display that you want to have on all the time.
You might want to have some sort of daemon running on the display so that you can control it from elsewhere.
To do this we can use [v3x-mission-control](https://github.com/v3xlabs/mission-control).

Mission Control is a lightweight rust daemon that allows you to expose your displays backlight, brightness, and chromium controls to [homeassistant](https://www.home-assistant.io/) via the [mqtt integration](https://www.home-assistant.io/integrations/mqtt/).
