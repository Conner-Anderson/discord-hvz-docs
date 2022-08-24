# Setup



## Guild Setup

### Channels

Your Discord guild must have the `channels names` section configured in `config.yml` to function. Please see [Config Options: channel names](../config_options/#channel_names) for information. You can change these channels later, but **the bot must have permission to view and post in these channels.**

To play the game, guild members need a way to click the registration button, and zombies need a way to click the tag logging button. Let's create those now. 

Go the channel you want the registration button in. It is recommended that this channel be one dedicated to information about the game. 
Send the command [`/post_button`](../commands/#post_button) and selection "registration" as the `button_1` option. 

If you're unfamiliar with Discord Slash Commands, look here: 

!!! info

    If you don't like where the registration button sits in the channel, you can always delete it and try again later. The buttons are quite forgiving.

Do the same for the `tag_logging` button in the same channel you configured as the [`report-tags`](../config_options/#report-tags) channel. You can get creative and post this button in other places too, but be warned: **anyone who can click the button can try to log a tag, even humans.** It is designed this way so *you* are responsible for setting up the guild roles, permissions, and channels to keep humans and zombies where you want them.

**Setup admin roles**

### Roles

Yoyr Discord guild must have the `roles`

