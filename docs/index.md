
![Hero](img/hero4.webp)

# Documentation for Discord-HVZ

Discord-HVZ is a run-it-yourself Discord bot that helps run **Humans vs. Zombies** games. This documentation contains everything you need to manage an HVZ game with the help of [Discord](https://discord.com/).

For information about HvZ: [Humans vs Zombies on Wikipedia](https://en.wikipedia.org/wiki/Humans_vs._Zombies) 

For official releases and to see the source code, see the [:simple-github: GitHub](https://github.com/Conner-Anderson/discord-hvz).

Need help setting up and running an HvZ game, either with this bot or in general?  
Contact me at *conneranderson.dev@gmail.com*

## Summary

This bot was developed to run HvZ games at LeTourneau University in Texas, but is now published for use anywhere. A version of it has been used each semester at LeTourneau since Fall 2019. It is a *simple and streamlined* system designed to leverage the easy-to-use features of Discord which already manages accounts, permissions, and messaging. It was developed in response to the popular service *HvZ SOURCE* going offline.

### Features

- Registers players for the game through a questionnaire
- Accepts tag reports from players by validating tags, changing player roles, and announcing the tag to all players
- Displays the zombie tag geneology and other statistics
- Displays player and tag information in a live Google Sheet for easy administration
- Fully supports both desktop and mobile users
- Includes many commands for managing the game
- Includes configuration options and customizable questionnaires
- Works on existing Discord servers without disrupting non-players
- Works on :material-microsoft-windows: Windows and :material-linux: Linux, with :material-apple: Mac coming soon.
- Written in Python, potentially aiding personalization

### Prerequisites


- Mild tech-saviness. You must not be afraid of a command prompt or editing configuration files.
- A Discord account that owns the server you want to run the bot on. You could also just get the owner to do a few setup steps for you.
- A Google account to access the Google Sheet and the Sheets API.
- A computer with an internet connection that can run the bot for the entire game. For example, you may leave your desktop on, borrow someone's old laptop, or use a Raspberry Pi in the closet. 

This software is for HvZ admin teams willing to do a bit of legwork to set it up, giving you an HvZ system entirely under your control.

## Get Started


[Installation](installation.md){ .md-button } &emsp; &emsp; &emsp; &emsp; Start from scratch in getting the bot running.

[Running the Game](running_the_game.md){ .md-button } &emsp; Learn the process of running a game with Discord-HvZ.

[Commands](commands.md){ .md-button } &emsp; &emsp; &emsp; &emsp; Reference the bot's commands.




## Development

Hey! I'm Conner, and I wrote this bot. I did it to support the game I love at my alma-mater and also to learn how to write better software. If people actually use this to run HvZ games, I'll keep adding features and fixing bugs!
