# Config Options

## `channel_names`

These settings map specific channels the bot needs to the actual channel names on the guild. If these channels aren't specified, the bot will search for the channels that match the names below. The bot will fail to start if it doesn't find a channel.

#### `tag-announcements` 
The channel where tags are announced. It is recommended that all players can view this, but only admins and the bot can post.
#### `report-tags` 
A channel that the tag logging button is posted in. Only zombies should have permission to view this. The bot needs to know about this channel so it can secretly give the OZs access to it.
#### `zombie-chat`
A channel zombies can use to talk to each other in, which only they have access to. The bot needs to know about this channel so it can secretly give the OZs access to it.
#### `bot-output`
The channel the bot can use to output system messages to. Should only be visible to admins, and should be muted.