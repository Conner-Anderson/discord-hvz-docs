# Commands

## Overview

Discord's Slash Commands provide a robust way to control the bot. You can start composing a command by typing `/` in any channel the bot has access to, which will bring up a list of available commands. Finish typing what you want, or select the command in the list.

Commands are grouped into categories such as `member` and `tag` to stay organized. For example, to list members, use the sub-command of `member` like this: `/member list`. 

Most commands have arguments to supply, which Discord will prompt you to do fill-in-the-blank style. If asked to supply a member, you can start typing their name to select from a list. Most arguments will tell you what kind of response they expect, and if they are optional.

In this documentation, arguments for commands are annotated like this:

`<input>` means the argument is required, and you supply the contents.

`[input]` means the argument is optional, and you supply the contents.

`input` means you need to type "input" exactly as shown.

`...` means there are some arguments not listed that you can infer logically.

## Standalone Commands

### code
`/code`

Replies to a player with their own tag code in an "ephemeral" message only they can see, in case they lose it. This tag code is needed for zombies to log their tag.

### config
`/config <setting> [choice]`

Manipulates certain important configuration settings. Displays the current setting if no `choice` is given, and changes the setting if it is.
**Add settings here**


### oz
`/oz <member> [setting]`

Reports whether or not a member is an OZ (original zombie), and changes their OZ status if `setting` is supplied.

An OZ is defined as a player who is set as one in the database. This status has no bearing on the game, but is useful for tracking purposes.

When an OZ is added with this command, they are given specific permission to access the [zombie-chat](../config_options/#zombie-chat) and [report-tags](../config_options/#report-tags) channels. This permission is invisible to other server members, and so can be kept secret. When an OZ is removed, these specific permissions are removed as well.

You don't need OZs to run the game. Manually giving a player (even a human) access to the tag logging button is enough to get things started.

See the [silent-oz](../config_options/#silent-oz) config option.

### post_button
`/post_button [text] [button_1] [button_2] ... [button_5]`

Sends a message in this channel that contains one or multiple buttons that will start one of the chatbots. It will also contain text if you choose.
There is no limit to the number of the same button that can exist in a guild, and they can be deleted at any time.

By default, the bot comes with two chatbot buttons: registration and tag_logging.

### post_panel
`/post_panel <element1> [element 2] ... [element 6] [static]`

A *panel* is a special message with attached game information, each of which is called an *element*. A panel can have multiple elements, such as the number of humans, number of zombies, players registered today, etc. There's a special element called `GamePlotElement` which displays a plot of human and zombie populations over the course of the game.

The `static` option is by default `false`, which means the panel will keep its values up to date with the current game. If `true`, it will never change.

!!! bug
    The `GamePlotElement` is a complex and really cool plot that has some issues right now. Check first to see if it works before showing players.

### shutdown
`/shutdown [force]`

Shuts down the bot, which must be restarted from the computer it is running on. If there are any chatbots running, they will be listed, and the bot will not shutdown. If `force` is supplied as `true`, the bot will shut down anyways, closing any chatbots. The members using those chatbots will be notified, and will need to start over.

## Member Commands

### member list
`/member list`

Lists all members. Generally, looking at the Google Sheet is a better option.

### member register
`/member register <member>`

Starts a registration chatbot with you on behalf of the selected member. You will answer all questions, but the member you selected will be actually registered. Helpful if the chatbot is acting up for someone, or they were accidentally deleted from the game.

### member delete
`/member delete <member>`

Removes the selected member from the game. They remain on the server and in tag records, but are removed from as a member from the database and have their game roles revoked. Will probably mess up some game statistics...

!!! bug
    Cannot currently delete members no longer on the server.

### member edit
`/member edit <member> <attribute> <value>`

An advanced command to directly edit a member in the database. You should consult the Google Sheet before using this.

`attribute` must exactly match the database column to change, as seen on the Google Sheet.

`value` is the new value. The command does not check if this is a valid value!

!!! warning
    This command does not yet support changing dates. It might work... but it might not. Use a database editor like [DB Browser for SQLite](https://sqlitebrowser.org/) to change dates until this feature is added.


## Tag Commands

### tag create
`/tag create <member>`

Starts a tag logging chatbot with you on behalf of the selected member. You will answer all questions, but the member you selected will actually make the tag. Helpful if the chatbot is acting up for someone, or if they just can't get to a device.

### tag delete
`/tag delete <tag_id>`


Deletes a tag log by its `tag_id`, as viewable on the Google Sheet. If the target of the deleted tag has no more tags against them, they are reverted to human. Remember, if you don't like this functionality, you can always swap their roles back via Discord.

### tag revoke
`/tag revoke <tag_id>`

Identical in behavior to `tag delete`, except instead of removing the tag, it is labeled as *revoked*. Statistics reporters such as `tag tree` will ignore the tag, and so can you when reading the Google Sheet. 

This is a more elegant way to remove a tag during a questionable dispute.

### tag restore
`/tag restore <tag_id>`

Restores a tag that has been revoked. Turns the tagged member back into a zombie. 

### tag edit
`/tag edit <member> <attribute> <value>`

An advanced command to directly edit a tag in the database. You should consult the Google Sheet before using this.

`attribute` must exactly match the database column to change, as seen on the Google Sheet.

`value` is the new value. The command does not check if this is a valid value!

!!! warning
    This command does not yet support changing dates. It might work... but it might not. Use a database editor like [DB Browser for SQLite](https://sqlitebrowser.org/) to change dates until this feature is added.

### tag tree
`/tag tree`

Trust me, this is your players' favorite command. It will post a geneology of zombies, showing who tagged whom. If the message overflows the 2000 character limit for messages, the message paginate with buttons to navigate the pages.

!!! warning
    If the pagination feature is needed, the pages will not be navigable after the bot restarts. This is a potential thing to fix in the future.


## Item Commands

!!! info
        The item tracking system is new and quite basic right now. Consider it in beta.


### item create
`/item create <name> [starting_player]`

Creates an item with the specified name. This name can be anything you like. By default, this item is placed into "storage", which means it isn't assigned to anyone. You can provide a `starting player` to give it to immediately.

### item list
`/item list [member]`

Lists all items in the game, their ids, and their owners. If you provide `member`, it will show only the items owned by that member.

### item transfer
`/item transfer <id> <target_player>`

Transfers an item to a player, regardless of who currently owns it. Use `item list` to find the right id.

### item take
`/item take <id>`

Moves an item from a player to storage.

### item rename
`/item rename <id> <new_name>`

Renames an item.

### item delete
`/item delete <id>`

Deletes an item. There is no undo.

### item delete_all
`/item delete_all <are_you_sure>`

Completely deletes all items. There is no undo!
