# broadcast

This is a simple Linux utility, written in Bash, to run [`notify-send`](https://gitlab.gnome.org/GNOME/libnotify) on all users.

It includes the optional ability to queue notifications for offline individual users, that will be shown when they log in.

Notifications can also be sent to administrators only, depending on the given configuration.


## Usage

```txt
# broadcast [a[a]][q][v] ARGUMENTS...
```

Where `ARGUMENTS...` are the usual arguments and options passed to `notify-send`. (See `notify-send(1)`).

The letters in the first (optional) argument to `broadcast` are flags that modify its behavior. They are as follows:

- `a` --- Send the notification to administrators only. Administrators are users belonging to any group listed in `ADMIN_GROUPS` in the config. If specified twice (`aa`), users belonging in `ADMIN_EXTRA_GROUPS` will receive the notification as well.

- `q` --- Queue notifications for offline users.

- `v` --- Enable verbose output.

### Examples

```txt
# broadcast --urgency=normal 'This is a summary!' 'This is the notification body.'
```

```txt
# broadcast q "If you're reading this right upon logon that means you've probably missed something important."
```

### Configuration

The configuration file is located at `/etc/broadcast.conf` and is self-documenting.

### Queueing Notifications

The `broadcast-dequeue` command is provided for users to receive missed notifications that were sent (with the `broadcast` `q` flag) when they were offline. A `broadcast-dequeue.service` systemd user unit is also included, that will run `broadcast-dequeue` upon logging into a graphical session.

The usage of `broadcast-dequeue` is described as follows:

```txt
$ broadcast-dequeue [v]
```

The `v` flag enables verbose output if supplied.

Queued notifications are stored in the file defined by `QUEUE_FILE_PATH` in the config. This defaults to `~/.broadcast`.


## Dependencies

- `notify-send` from libnotify, for sending the actual notifiations
- `sudo`, for executing `notify-send` as each target user
- `pgrep`, for getting currently running D-Bus session daemons


## Contributing

Feel free to submit bug fixes and feature( request)s.
