# Cheatsheet

## Linux

### Ubuntu

#### Shut down the computer

```bash
sudo shutdown now
sudo poweroff
sudo halt -p
```

## MacOS

### ESC

To invoke Escape, press Command + period (.) on the keyboard.

## snap

### remove disabled snaps

```sh
#!/bin/bash

# Get a list of all installed snaps with the "disabled" note
disabled_snaps=$(snap list --all | grep "disabled" | awk '{print $1}')

# Loop through each disabled snap
for snap in $disabled_snaps; do
    # Get a list of revisions for the disabled snap
    revisions=$(snap list --all "$snap" | grep "disabled" | awk '{print $3}')

    # Remove all revisions of the disabled snap
    for revision in $revisions; do
        if [ "$revision" != "-" ]; then
            sudo snap remove "$snap" --revision="$revision"
        fi
    done
done
```

### Close dead ssh connection

[how to exit broken ssh session](https://discussion.fedoraproject.org/t/howto-exit-bad-broken-locked-ssh-sessions/68608)

> you can just close the SSH session with an escape sequence: next time, just do Enter + ~ + . ! So just press the enter key, followed by the tilde key followed by the dot - and you should be back to your terminal!
