# scli
`scli` is a simple terminal user interface for [Signal](https://signal.org). It uses [signal-cli](https://github.com/AsamK/signal-cli) and [urwid](http://urwid.org/).

# Installation
- Firstly, you need to install [signal-cli](https://github.com/AsamK/signal-cli). Follow the guide provided in the README.
- Install `libunixsocket-java` from your package manager if you have not installed it yet. (Arch Linux users should install `libmatthew-unix-java` from AUR. If you installed `signal-cli` from AUR, you can skip this step.)
- Install `urwid`. You can install it trough your distributions package manager (search for `python3-urwid` or `python-urwid`) or you can use `pip` to install: `pip3 install urwid`.

## Linking your device and using
`scli` does not provide anything for registering/linking, you need to do this using `signal-cli`.

For linking your computer follow these steps (for registering a new number, see README of [signal-cli](https://github.com/AsamK/signal-cli))
- Run `signal-cli link -n "DEVICE_NAME"`. (DEVICE_NAME is just an alias for your computer. You can skip `-n "DEVICE_NAME` part. Without that, your devices alias will be just `cli`.)
- This will output `tsdevice:/...` URI (Do not terminate this command, it needs to be alive during linking process. Continue in a separate terminal.). Copy the URI and create a QR code with it using `qrencode` (or any other QR code generator of your choice):
```
qrencode 'LINK' -o qrcode.png
```
- Open Signal application on your phone and scan the QR code you just generated.
- Run `signal-cli -u PHONE_NUMBER receive`. This is required to fetch your contacts for the first time.
- Now you can start using:
```
scli
```

**Note**: `PHONE_NUMBER` starts with `+` followed by the country code.

# Usage
A simple two-paned interface is provided. Left pane contains the contact list and the right pane contains the conversation. You can switch focus between panes by hitting `Tab` (or `Shift + Tab`). Hitting tab for the first time focuses the conversation, hitting it again focuses to input line. So the tab order is `Contacts -> Conversation -> Input`, you can use `Shift + Tab` for cycling backwards.

## Keys
- Use `j`/`k` to go down/up in contacts list or in messages.
- Hitting `enter` on a contact starts conversation and focuses to input line.
- Hitting `l` on a contact only starts conversation.
- Hitting `o` on a message opens the URL if there is one, if not it opens the attachment if there is one.
- Hitting `enter` on a message opens the attachment if there is one, if not it opens the URL if there is one.
- Hitting `y` on a message puts it into system clipboard. (needs `xclip`)
- `g` focuses first contact/message.
- `G` focuses last contact/message.
- `d` deletes the message from your screen (and from your history, if history is enabled).
- `i` show a popup that contains detailed information about the message.

## Commands
There are some basic commands that you can use. Hit `:` to enter command mode (or simply focus the input line and type `:`).

- `:quit` or `:q` simply quits the program.
- `:openUrl` or `:u` opens last URL in messages, if there is one.
- `:openAttach` or `:o` opens last attachment in messages, if there is one.
- `:attach FILE_PATH` or `:a FILE_PATH` attaches given file to message.
- `:attachClip` or `:c` attaches clipboard content to message. This command tries to detect clipboard content. If clipboard contains something with the mime-type `image/png` or `image/jpg`, simply attaches the image to message. If clipboard contains `text/uri-list` it attaches all the files in that URI list to your message. This command needs `xclip` installed.
- `:toggleNotifications` or `:n` toggles desktop notifications.
- `:edit` or `:e` lets you edit your message in your `$EDITOR`.
- `:toggleAutohide` or `:h` toggles autohide property of the contacts pane.

Examples:
```
:attach ~/cute_dog.png check out this cute dog!
:attachclip here is another picture.
```
**Note**: Commands are case insensitive, `:quit` and `:qUiT` does the same thing.

## Searching
There is a built-in search feature. Simply hit `/` (or focus the input line then type `/`) and start typing, the chat will be filtered out based on your input. You can focus any of the search results and hit `enter` (or `l`) to open that result in full conversation.

## Configuration
There are some simple configuration options. You can either pass them as command-line arguments or add them to your configuration file. Run `scli --help` to see options. Configuration file syntax is also pretty easy. Lines starting with `#` and empty lines are ignored, other lines should consist `key=value` pairs.

### Example
```sh
scli -u +1234567890 --enable-notifications=true
```
Configuration file equivalent of this command is:
```ini
# Long option forms are used in config file. (u=+123... is not valid.)
username=+1234567890
enable-notifications=true
```

# Screenshots
![scli](screenshots/1.png?raw=true)
![scli](screenshots/2.png?raw=true)
![scli](screenshots/3.png?raw=true)
