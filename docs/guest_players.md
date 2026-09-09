# Players without Discord Accounts

Some people want to play HvZ but don't have a Discord account. Register them as *guests*! They get tag codes just like everyone else, and someone you trust can use the server to report their tags for them.

Guests appear by name in the [tag tree](commands.md#tag_tree), count toward the game statistics and [panels](commands.md#post_panel), and appear in the Google Sheet. If a Discord player tags a guest, who then tags another player, the whole chain is preserved.

## Give Someone Permission

There are two commands to give people access to:

- `/guest-admin` is for registering and managing guests. Your moderators will probably need this.
- `/guest-tag` is for reporting tags made by guests. Give this to whichever people you trust to help them.

**Both commands are restricted to administrators by default.** To let someone else use them, go to `Server Settings / Integrations / [Your Bot]`, select the command, and allow the appropriate roles or members. See [Server Setup: Commands](server_setup.md#commands) for more about these permissions.

You can give someone permission to report guest tags without letting them register or edit guests. The commands within `/guest-admin`, however, all share the same permissions.

A person with `/guest-tag` permission can report for **any guest**. There is no need to assign them to particular people. They don't even have to be registered for the game themselves. Their own human or zombie status doesn't matter: the guest is the one making the tag.

## Register a Guest

Use [guest-admin register](commands.md#guest-admin-register) and supply a name. For example:

`/guest-admin register name:Jamie`

The bot replies with Jamie's tag code, player ID, and faction in a message only you can see. Give Jamie the tag code before they start playing. A paper card or a code written on a bandana works just as well as it does for your Discord players.

Choose a distinct display name for each guest so the people reporting tags can tell them apart. If someone loses their code, use [guest-admin info](commands.md#guest-admin-info) to retrieve it, or [guest-admin list](commands.md#guest-admin-list) to see all the guests.

!!! info
    Guest registration only asks for a name and whether the guest is an OZ. It doesn't ask the questions from your registration chatbot, such as email or bandana preferences. Collect any other information you need yourself. Moderators can register guests even when ordinary [registration](config_options.md#registration) is disabled.

### Guest OZs

If Jamie should start as an original zombie, supply `oz:true` when registering:

`/guest-admin register name:Jamie oz:true`

You can also change this later with [guest-admin oz](commands.md#guest-admin-oz). A guest OZ can make tags immediately, and appears in the tag tree even before making their first tag.

With [silent_oz](config_options.md#silent_oz) enabled, original zombies are counted as humans in the public population totals and the historical graph. This doesn't stop a guest OZ from making tags.

## Log Tags

### When a Guest Gets Tagged

The zombie takes the guest's tag code and reports it with the usual tag logging button. Nothing special is needed! The guest is changed to zombie in the game records, and the tag is announced as usual.

If the zombie is also a guest, someone with `/guest-tag` permission reports it using the process below.

### When a Guest Tags Someone

The guest collects the tagged player's code and tells their helper the code and approximately when the tag happened. The helper then uses [guest-tag](commands.md#guest-tag). For example:

`/guest-tag guest:Jamie tagged_code:ABCDEF time:3:04pm`

Here, Jamie is the zombie who made the tag, and `ABCDEF` is the code of the person Jamie tagged. The tagged person can be another guest or a Discord player.

You can identify Jamie by name, player ID, or tag code. Leave out `time` to use the current time, or supply a time in the game's configured timezone. For a tag from the previous day, use something like `3:04pm yesterday`.

The bot credits the tag to Jamie. It also records who submitted it in the Google Sheet's `reporter_id` column, so moderators can follow up if something needs correcting. Reporting for Jamie doesn't add a tag to the helper's own record or change their faction.

!!! note
    The guest must be a zombie, the tagged player must still be human, and [tag_logging](config_options.md#tag_logging) must be enabled. A guest cannot tag themselves. Valid reports are saved immediately; they do not wait for a moderator to approve them.

## Make Corrections

Use the normal [tag revoke](commands.md#tag-revoke), [tag restore](commands.md#tag-restore), and [tag delete](commands.md#tag-delete) commands for tags involving guests. The guest's faction updates along with the tag record. An OZ, or a player with another active tag against them, remains a zombie when one tag is removed.

Use [guest-admin rename](commands.md#guest-admin-rename) if a guest's name needs changing. Their code and tag history stay connected to the same player. Guests with tag history cannot be deleted with `/member delete`, since that would break the chain. Rename them instead.

Guest records and their tag codes survive bot restarts. You don't need to register them again each time you run the bot.
