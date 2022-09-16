# Setup



## Server Setup

### Channels

Your Discord server must have the `channel_names` section configured in `config.yml` to function. Please see [Config Options: channel_names](../config_options/#channel_names) for information. You can change these channels later, but **the bot must have permission to view and post in these channels.**

To play the game, server members need a way to click the registration button, and zombies need a way to click the tag logging button. Let's create those now. 

Go the channel you want the registration button in. It is recommended that this channel be one dedicated to information about the game. 
Send the command [`/post_button`](../commands/#post_button) and selection "registration" as the `button_1` option. 

If you're unfamiliar with Discord Slash Commands, look here: [Commands Overview](../commands/#overview)

!!! info

    If you don't like where the registration button sits in the channel, you can always delete it and try again later. The buttons are quite forgiving.

Do the same for the `tag_logging` button in the same channel you configured as the [`report-tags`](../config_options/#report-tags) channel. You can get creative and post this button in other places too, but be warned: **anyone who can click the button can try to log a tag, even humans.** It is designed this way so *you* are responsible for setting up the server roles, permissions, and channels to keep humans and zombies where you want them.

It is also recommended that you create a channel purely for sending the bot commands, and leave it muted. No need to clutter up other channels with that. Remember to give the bot access to it.

**Setup admin roles**

### Roles

Your Discord server must have the `roles` section configured as well. See [Config Options: role_names](../config_options/#role_names) for information.
Ensure you have a role for each faction, and one for all players. **The bot must have permission to manage these roles**, meaning they have to be lower than it in the role hierarchy. 

### Commands

Discord lets you manage command permissions through `Server Settings / Integrations / [Your Bot]`. Here you can give permissions to certain members and roles to use categories of commands. **By default, everyone can use everything.** That's bad, so restrict most commands to only your admins. The `/code` command is designed to be used by players though.

