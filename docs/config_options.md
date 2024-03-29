# Config Options

### Summary

Most configuration for the bot is done in the `config.yml` file. This is written in YAML format which is made to be very readable. The config file includes helpful explanations within to supplement the information on this page. While you can technically edit it in any text editor, it's more sane to use one designed for code like [Notepad++](https://notepad-plus-plus.org/downloads/) for Windows that will make the whole experience nicer.

When editing the file, your goal should be to *follow the pattern*. Extra spaces, tabs, and punctuation will likely cause the bot to give an error. In general, never change the text to the left of a colon, as this is the name of a variable the bot is looking for. The exception to this is when you are adding entirely new lines, such as for `database_tables`. 

Don't make the name of something just a number. It might technically work, but you're risking the bot mis-interpreting. So don't be cheeky and name your Google Sheet for tags `69`. Likewise, other clever names with odd characters are just not good form. Let text be text here.

### `server_id`

The ID of the Discord server to connect to. This can be found at the [Discord Developer Portal](https://discord.com/developers/applications){target=_blank} under General Information for your application.

### `sheet_id`

The ID of the Google Sheet the bot will send its data to. See [Setting Up Google Sheets](../installation/#setting-up-google-sheets) for more information.

### `registration`

When `true`, the registration button will launch the chatbot. When `false`, it will not, and so closes registration.

### `tag_logging`

When `true`, the tag_log button will launch the chatbot. When `false`, it will not, and so closes tag logging.

### `silent_oz`

When `true`, the identity of an [OZ](../commands/#oz) is not included in tag announcements. This lets you keep who the OZs are a mystery until players see them in the real world. You will likely want to turn this `False` once the OZs are commonly known.

### `google_sheet_export`

When `true`, any changes to the database will be pushed out to the Google Sheet. The bot is designed to use the Google Sheet, so it is not recommended to go without it. If you are having trouble with the Google Sheet and you just need the bot to work, set this to `false`. As a backup, you can still read the database with an editor such as [DB Browser for SQLite](https://sqlitebrowser.org/).


### `sheet_names`

These are the names of the sheets within the Google Sheets document that data will be sent to. The variables here match the names of the tables in the database.

!!! example
    ``` yaml
    sheet_names:
    	members: Players_Live
    	tags: Tags Live
    ```

### `timezone`

The timezone *the game* is occuring in. You bot can run in a different timezone and still report accurate times. Surround this in quotes.

For the best timezone tracking, make this value your location. Find the IANA database name for your location here: [Wikipedia: List of database timezones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

Alternately, use a number representing the timezone the game is occuring in as an offset from UTC. -7 is US West Coast, -5 is US Central, -4 is US East Coast, Central Europe is +2

!!! example
	``` yaml
	timezone: 'America/Chicago'
	```
	``` yaml
	timezone: "+5"
	```
	``` yaml
	timezone: "Europe/Warsaw"
	```

### `channel_names`

These settings map specific channels the bot needs to the actual channel names on the server. If these channels aren't specified, the bot will search for the channels that match the names below. The bot will fail to start if it doesn't find a channel.

!!! example
	``` yaml
	channel_names:
		tag-announcements: zombie-kills
		report-tags: report-tags-here
		zombie-chat: zombie-lair
	```

#### `tag-announcements` 
The channel where tags are announced. It is recommended that all players can view this, but only admins and the bot can post.
#### `report-tags` 
A channel that the tag logging button is posted in. Only zombies should have permission to view this. The bot needs to know about this channel so it can secretly give the OZs access to it.
#### `zombie-chat`
A channel zombies can use to talk to each other in, which only they have access to. The bot needs to know about this channel so it can secretly give the OZs access to it.


### `role_names`

These settings map specific roles the bot needs to the actual role names on the server. If these roles aren't specified, the bot will search for the roles that match the names below. The bot will fail to start if it doesn't find a role.

!!! example
	``` yaml
	role_names:
		zombie: Brain Eater
		human: Brain-Box
		player: Player
	```

#### `zombie` 
Tagged players will get this role and should have access to click the tag logging button.
#### `human` 
Tagged players will lose this role and should not have access to click the tag logging button, except for secret OZs.
#### `player`
A role all registered players get. Good for separating game channels from other channels in the guild. 


### `database_tables`

This is a sophisticated set of config options that allows you to define the following:

- What tables the database has
- What columns each table has
- What type of data each column stores
- The order the columns appear on the Google Sheet

A table is a 2-dimensional array of data: like a spreadsheet. Each row is an entry (such as a member or a tag) and each row has the same columns. The bot's database, stored as the file `hvzdb.db`, contains both tables defined here and behind-the-scenes tables you can't control. The tables defined here collect data from [chatbots](../running_the_game/#chatbots).

The order the columns appear in the config is the order they will appear on the Google Sheet.

The bot requires the following tables, columns, and types to function:

``` yaml
database_tables:
  members:
    id: String
    name: String
    faction: String
    tag_code: String
    want_bandana: String
    registration_time: DateTime
    oz: Boolean
  tags:
    tag_id: incrementing_integer
    tagger_id: String
    tagger_name: String
```

If any of these are missing from the config, the bot will create them anyways and position them last on the Google Sheet.

### `database_path`

The path to the database file. If the file does not exist, the bot will create it. Regardless of which system the bot runs on, use `/` forward slashes in the path. The bot will search for the file in the top directory: the one that contains config.yml, .env, etc.  
If the path is a file, it must end in `.db`. If it is a folder, it must already exist, and a file called `game_database.db` will be created inside.  
If the path starts with a `/`, it begins at the root of the drive. On Windows, `/Users/JohnDoe` would go to John Doe's user folder.

!!! example "`database_path` Examples"
	```yaml
	database_path: "my_database_name.db"
	```
	```yaml
	database_path: "database_storage/"
	```
	```yaml
	database_path: "my_databases/spring_2025_database.db"
	```
	```yaml
	database_path: "/Users/JohnDoe/Documents/HvZ-Databases/game_database-2023.db"
	```