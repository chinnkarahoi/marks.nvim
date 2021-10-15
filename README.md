# marks.nvim
A better user experience for interacting with and manipulating Vim marks.
Requires Neovim 0.5+.

![](../assets/marks-demo.gif)

Screenshot:

![](../assets/demo_screenshot.png)

## Features

- view marks in the sign column
- quickly add, delete, and toggle marks
- cycle between marks
- preview marks in floating windows
- extract marks to quickfix/location list
- set bookmarks with sign/virtual text annotations for quick navigation across buffers

## Installation

I recommend you use your favorite vim plugin manager, like vim-plug, or packer.

For example, using vim-plug, you would add the following line:

`Plug 'chentau/marks.nvim'`

If you want to manually install, you can clone this repository, and add the path
to the cloned repo to your runtimepath: `set rtp+=/path/to/cloned/repo`.

## Setup

```lua
require'marks'.setup {
  default_mappings = true, -- whether to map keybinds or not. default true
  builtin_marks = { ".", "<", ">", "^" }, -- which builtin marks to show. default {}
  cyclic = true, -- whether movements cycle back to the beginning/end of buffer. default true
  force_write_shada = false, -- whether the shada file is updated after modifying uppercase marks. default false
  refresH_interval = 250, -- how often (in ms) to redraw signs/recompute mark positions. 
                          -- higher values will have better performance but may cause visual lag, 
                          -- while lower values may cause performance penalties.
  bookmark_0 = { -- marks.nvim allows you to configure up to 10 bookmark groups, each with its own sign/virttext
    sign = "⚑",
    virt_text = "hello world"
  },
  mappings = {}
}
```

See `:help marks-setup` for all of the keys that can be passed to the setup function.

## Mappings

The following default mappings are included:

```
    mx              Set mark x
    m,              Set the next available alphabetical (lowercase) mark
    m;              Toggle the next available mark at the current line
    dmx             Delete mark x
    dm-             Delete all marks on the current line
    dm<space>       Delete all marks in the current buffer
    m]              Move to next mark
    m[              Move to previous mark
    m:              Preview mark. This will prompt you for a specific mark to
                    preview; press <cr> to preview the next mark.
                    
    m[0-9]          Add a bookmark from bookmark group[0-9].
    dm[0-9]         Delete all bookmarks from bookmark group[0-9].
    m}              Move to the next bookmark having the same type as the bookmark under
                    the cursor. Works across buffers.
    m{              Move to the previous bookmark having the same type as the bookmark under
                    the cursor. Works across buffers.
    dm=             Delete the bookmark under the cursor.
```

Set `default_mappings = false` in the setup function if you don't want to have these mapped.

You can change the keybindings by setting the `mapping` table in the setup function:

```lua
require'marks'.setup {
  mappings = {
    set_next = "m,",
    next = "m]",
    preview = "m;",
    set_bookmark0 = "m0",
    prev = false -- pass false to disable only this default mapping
  }
}
```

The following keys are available to be passed to the mapping table:

```
  leader                 Prefixes all commands. also handles setting and deleting named marks.
  set_next               Set next available lowercase mark at cursor.
  toggle                 Toggle next available mark at cursor.
  delete_line            Deletes all marks on current line.
  delete_buf             Deletes all marks in current buffer.
  next                   Goes to next mark in buffer.
  prev                   Goes to previous mark in buffer.
  preview                Previews mark (will wait for user input). press <cr> to just preview the next mark.
  set                    Sets a letter mark (will wait for input). the leader key implements this functionality by default,
                         so you only need to set this if you disable the leader (see below)
  delete                 Delete a letter mark (will wait for input). just like 'set', this is automatically handled if
                         the leader key is present, so only set this if the leader is disabled.
                
  set_bookmark[0-9]      Sets a bookmark from group[0-9].
  delete_bookmark[0-9]   Deletes all bookmarks from group[0-9].
  delete_bookmark        Deletes the bookmark under the cursor.
  next_bookmark          Moves to the next bookmark having the same type as the
                         bookmark under the cursor.
  prev_bookmark          Moves to the previous bookmark having the same type as the
                         bookmark under the cursor.
  next_bookmark[0-9]     Moves to the next bookmark of of the same group type. Works by
                         first going according to line number, and then according to buffer
                         number.
  prev_bookmark[0-9]     Moves to the previous bookmark of of the same group type. Works by
                         first going according to line number, and then according to buffer
                         number.

```

marks.nvim also provides a list of `<Plug>` mappings for you, in case you want to map things via vimscript. The list of provided mappings are:

```
<Plug>(Marks-set)
<Plug>(Marks-setnext)
<Plug>(Marks-toggle)
<Plug>(Marks-delete)
<Plug>(Marks-deleteline)
<Plug>(Marks-deletebuf)
<Plug>(Marks-preview)
<Plug>(Marks-next)
<Plug>(Marks-prev)

<Plug>(Marks-delete-bookmark)
<Plug>(Marks-next-bookmark)
<Plug>(Marks-prev-bookmark)
<Plug>(Marks-set-bookmark[0-9])
<Plug>(Marks-delete-bookmark[0-9])
<Plug>(Marks-next-bookmark[0-9])
<Plug>(Marks-prev-bookmark[0-9])
```

See `:help marks-mappings` for more information.

## Highlights and Commands

marks.nvim defines the following highlight groups:

`MarksSignHL` The highlight group for displayed mark signs.

`MarkNumSignHL` The highlight group for the number line in a signcolumn.

`MarkVirtTextHL` The highlight group for bookmark virtual text annotations.

marks.nvim also defines the following commands:

`:MarksToggleSigns` Toggle signs in the buffer

`:MarksListBuf` Fill the location list with all marks in the current buffer.

`:MarksListGlobal` Fill the location list with all global marks in open buffers.

`:MarksListAll` Fill the location list with all marks in all open buffers.

`:BookmarksList group_number` Fill the location list with all bookmarks of group "group_number".

`:BookmarksListAll` Fill the location list with all bookmarks, across all groups.


## See Also

[vim-signature](https://github.com/kshenoy/vim-signature)

[vim-bookmarks](https://github.com/MattesGroeger/vim-bookmarks)

## Todos

- Operator pending mappings and count aware movement mappings