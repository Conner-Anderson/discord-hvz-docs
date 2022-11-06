# Customized Chatbots

!!! warning "Under Construction"
    
    This page is a work-in-progress, and will inevitably change.

**Before reading this page, you should be familiar  
 with everything on the [Running the Game](../running_the_game) page.**

## Beginning

**Chatbots** follow **scripts** which are a series of **questions** with information on what to do with the answers. Scripts also have accompanying information on what to do with the entire set of answers, and what to say to the target member. Think of a script like the piece of paper a survey-taker would use.

Every script requires a **table** in the bot's database, where every row is one completed chatbot. Every question needs a column on the table to store the answer.

The two big, default, *required* chatbots are `registration` and `tag_logging`. Your HvZ game is unique, and requires questions phrased for your situation. By default, the registration script comes with a question for the member's CPO number (that's the school post-box address). Well, your game may not need that. This page will teach you how to remove that question, add new ones, and customize other things in these chatbot scripts.

Scripts are stored in the `scripts.yml` file, written in the YAML configuration language. It is recommended to use a code editor such as [Notepad++](https://notepad-plus-plus.org/downloads/) or one one of many other options.

If you want to do more than change the phrasing of something, read on.

**Coming Up**

- Edit script fields
- Add or remove questions
- Add chatbots

## Edit Script Fields

Changing text your users see in a chatbot is the easiest thing. Find a phrase in the script and edit it. Surround your text in single quotes if you need to use any of the following special characters: `:-{}[]!#|>&%@`, or if your text is only "yes", "no", "true", "false", or something similar.

See a `|-` in the file? That means what comes next is a multi-line piece of text that the bot should see as literally multiple lines.

If you are unsure about using unusual text, read [this reference.](https://docs.octoprint.org/en/master/configuration/yaml.html)

Below is an example of a functional tag_logging script similar to the one the bot comes with by default. You can use this as reference as you read onward.

!!! example "Example tag_logging Script"
    ```yaml
    tag_logging:
      table: tags
      beginning: Let's log a tag. Type 'cancel' at any time to stop this conversation.
      ending: Tag logged! Make sure your newly-brainless friend feels welcome in the hoard.
      starting_processor: tag_logging_start
      ending_processor: tag_logging_end
      questions:
      - column: tagged_id
        display_name: Tag Code
        query: What is the tag code of the human you tagged?
        valid_regex: '[a-zA-Z\d]{6}'
        rejection_response: 'The tag code is 6 characters of numbers and capital
          letters. None of these: 0 O Q I 1 D V U 5 S 2 Z'
        processor: tag_code_to_member_id
      - column: tag_time
        display_name: Tag Time
        query: About what time did the tag happen? Use the format 3:04pm. If it was yesterday, add "yesterday" to the end.
        valid_regex: '(?i)\d{1,2}:[0-5]?[0-9]\s?(am|pm)\s?(yesterday)?'
        rejection_response: Please use a format like 12:26pm or 7:00 am
        processor: tag_time
    ```

### Script Fields

??? info "Concise Reference."
    ``` yaml
    script_name: # Name of the script. Only for organization. Required, unique.
      table: members # Required, unique
      beginning: Registration started! # Message that appears when the chatbot starts. Optional.
      ending: Welcome to HvZ! # Message that appears when the chatbot ends. Optional.
      starting_processor: some_function # Described later in this document. Optional.
      ending_processor: another_function # Described later in this document. Optional.
      postable_button_color: green # Chatbot button color: green, red, blurple. Optional.
      postable_button_label: Click Me! # Visible label on the chatbot button. Optional.
      questions: # A list of the questions. Required.
      - column: favorite_color # The database column the answer will go in. No spaces, lower case. Required, unique per script.
        display_name: Favorite Color # The name of the question the user will see. Required.
        query: Favorite color? # The question text. Required.
        valid_regex: '\D*' # The regex the answer must satisfy. Optional.
        rejection_response: No numbers allowed in colors. # Response if the answer fails the regex test. Required if there is "valid_regex"
        processor: favorite_color # Described later in this document. Optional
    ```



#### `<script title>`
Required, unique

In the example above, the word `tag_logging` at the top declares that everything indented beneath it is the tag_logging script. If this name changes, any existing buttons for this chatbot will break.

#### `table`
Required, unique. 
Which table the script will be saved to. This table must be defined in the config file; see [Config Options: database_tables](../config_options/#database_tables)

#### `beginning`
Optional  
A message sent when the chatbot starts. It should inform the user that they can type "cancel" to stop the chatbot as well.

#### `ending`
Optional
A message sent when the chatbot ends. Preferably, it should tell the player what they ought to do next.

#### `starting_processor`
Optional  
The name of the starting processor function the script should use. Processors are described further below. If you make a new script and haven't made a new processor, this should be removed.

#### `starting_processor`
Optional  
The name of the ending processor function the script should use. Processors are described further below. If you make a new script and haven't made a new processor, this should be removed.

#### `postable_button_color`
Optional  
The color of the button used to start the chatbot. Valid values: **blurple, gray, green, red, url**. These colors are limited by Discord. Green by default.

#### `postable_button_label`
Optional  
The text that appears on the button used to start the chatbot. If not supplied, this will be the same as the script title.

#### `questions`
Minimum 1 question  
Questions are indented beneath this heading, marked with a `-` as shown in the example. They are asked in the order listed here.

### Question Fields

#### `column`
Required, unique per script  
The database column the question is saves to. When the bot is working with the question, this is effectively its "name". Remember, database column names should be lower case and without spaces. A name like "most_favorite_color" is fine.

#### `display_name`
Optional  
The human-friendly name users are showed when reviewing their responses.

#### `query`
Required  
The text of the question. Make sure it is concise, but unambiguous for best human results.

#### `valid_regex`
Optional  
Regex means "regular_expression", which is a special language for pattern matching. For example, `(?i)(yes|no)` is a set of coded symbols that tells the program it needs to match either "yes" or "no", ignoring case.

If `valid_regex` is provided, the bot will check the question's answer against the regex string, and if it matches *completely*, the answer is accepted. Note that any leading or trailing whitespace is stripped away from answers before this.

There are many resources online for building regex, including this one: [regex101.com](https://regex101.com/)

#### `rejection_response`
Required if `valid_regex` is supplied.  
The response the user is sent if their answer fails the regex test. They are given the opportunity to submit another response until they send a valid one.

#### `processor`
Optional  
Processors are an advanced feature described in their own section below. They can replace the functionality of regex, and make many checks that regex can't. They can customize their error responses to users, and transform the response into another form.

## Add or Remove Questions

To add a question, these are the basic steps:

- Add a new question section to a script in `scripts.yml` via copy-paste
- Edit the values to your liking, making sure to pick a new column name
- Add a new column to the [database_tables](../config_options/#database_tables) section of `config.yml`
- Delete your existing database file `hvzdb.db` and restart the bot, which will re-create it with your new column.
- Test the chatbot!

When using copy-paste to add a new question, make sure the indentation lines up. Indentation is made with spaces, not tabs.

When adding a database column to `config.yml`, you should set the type to `String` unless you are using processors.

!!! warning
    The bot will *not* add new columns to an existing database. This is called "migration" is and something the bot does not support for safety reasons. If you delete your database, the bot will create a fresh one that includes your new columns. If you need to add new columns without deleting your database, use a database editor such as [DB Browser for SQLite](https://sqlitebrowser.org/).

You ought to thouroughly test the new question to make sure it looks the way you want, especially if you are using regex or processor functions.

To remove a question, these are the basic steps:

- Delete the question section in its script in `scripts.yml`
- Optional: Remove the column from the database table in `config.yml`, then delete the database and restart the bot to remake it.

Having a database column with no corresponding question is perfectly fine, though it will still appear on the Google Sheet.

## Custom Processors

A script or question processor is a python function that, given a user response, determines how the bot should react. For example, when a player enters a tag code into the Tag Logging chatbot, the response is sent through a question processor function which finds a match for that code in the database. If there's a match, it returns the tagged player's ID which is saved as the question response. If there's no match, the player is prompted to try again. These processors could be as simple as duplicating the effects of regex, but are intended for creative uses.

!!! warning "Under Construction"
    Custom processors are a complex feature that could lead to many pitfalls. The documentation still needs to be written. For now, if you want custom functionality, contact the author of Discord HvZ.

