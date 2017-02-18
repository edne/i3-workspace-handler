# i3 Workspace Handler

Create i3 workspaces on the fly and call them by name.

![dmenu screenshot](https://i.imgur.com/kiVHu64.png)

Pressing `Mod+Space` (on my setup I press in sequence left and right thumbs) a
dmenu with the workspaces list appears, allowing to choose one.

Typing a non existing name a new one will be created.

With `Mod+Shift+Space` (left thumb, left pinkie, right thumb) you can move the
selected container.


## Installing

- Copy `lsws` in a folder in your __$PATH__, or read below to see what it does.

- Add these lines in your `.i3/config`:
 ```
bindsym $mod+space        exec i3-msg workspace $(lsws | dmenu -p "→")
bindsym $mod+Shift+space  exec i3-msg move container to workspace $(lsws | dmenu -p "|→")
bindsym $mod+u            workspace back_and_forth
```
  Eventually changing them with your Mod key and your colors and font
  configuration.


## Details

### lsws

List workspaces. As seen before we need to get the workspace names list in a
dmenu-friendly way, but `i3-msg -t get_workspaces` prints a json.

```bash
i3-msg -t get_workspaces |  # get the json
tr , '\n' |                 # replace commas with newline
grep name |                 # "name":"workspace-name"
cut -d \" -f 4              #         ^ 4th field, spliting by "
```

If you have it already installed you can use
[Jq](https://stedolan.github.io/jq/) to do the same job in a cleaner way:

```bash
i3-msg -t get_workspaces | jq -r ".[].name"
```

## License

Licensed under the Beerware License:
```
As long as you retain this notice you can do whatever you want with this stuff.
If we meet some day, and you think this stuff is worth it, you can buy me a
beer in return.
```
