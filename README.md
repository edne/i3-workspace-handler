# i3 Workspace Handler

Create i3 workspaces on the fly and switch (or move things) calling them by
name.

Pressing `Mod+Space` (on my setup I press in sequence left and right thumbs) a
dmenu with the list of current workspaces appears, allowing to choose one.
Typing a non existing name a new one will be created.

With `Mod+Shift+Space` (left thunb, left pinkie, right thumb) you can move the
selected container.


## Installing

- Copy `lsws` in a folder in your __$PATH__, or read below to see what it does.

- Add these lines in your `.i3/config`:
 ```
bindsym Mod1+space        exec i3-msg workspace $(lsws | dmenu -p "→")
bindsym Mod1+Shift+space  exec i3-msg move container to workspace $(lsws | dmenu -p "|→")
bindsym Mod1+u            workspace back_and_forth
```
  Eventually changing them with your Mod key and your colors and font
  configuration.


## Details

### lsws

List workspaces. As seen before we need to get the workspace names list in a
dmenu-friendly way, but `i3-msg -t get_workspaces` prints a json.

```
i3-msg -t get_workspaces |  # get the json
tr , '\n' |                 # replace commas with newline
grep name |                 # "name":"workspace-name"
cut -d \" -f 4              #         ^ 4th field, spliting by "
```