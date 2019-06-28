---
description: A useful and powerful feature.
---

# Scripting

Natsuko supports writing scripts to power its automatic moderation engine. Scripts are written in JavaScript. This allows you to, quite literally, teach the bot your server's rules. They are executed on message. Every object is wrapped in a Safe\* object, which can all be found in the [ScriptEngine package](https://github.com/natsuko-team/Natsuko/tree/master/src/ninja/natsuko/bot/scriptengine).

## Example: Naughty Word Filter

Let's say one of our rules is that you cannot say the phrase "naughty word". We can easily write a script for this like so:

{% code-tabs %}
{% code-tabs-item title="naughty-word-filter.js" %}
```javascript
if (message.getContents().contains("naughty word")) {
    message.delete();
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Alright, lets break this script down. `message` is the message that was sent. It is implemented in Java as a [SafeMessage](https://github.com/natsuko-team/Natsuko/blob/master/src/ninja/natsuko/bot/scriptengine/SafeMessage.java) object, which is a wrapper around a standard message. It has the following methods:

```javascript
message.getContents(); // returns a string containing the contents of the message
message.getTimestamp(); // returns a Java Instant object containing the message timestamp
message.getId(); // returns a Snowflake containing the ID
message.getAuthor(); // returns a SafeMember object which is the member that sent the message
message.getChannel(); // returns a SafeChannel object which is the channel the message was sent in
message.getGuild(); // returns a SafeGuild object which is the guild the message was sent in
message.mentionsEveryone(); // returns if the message mentions everyone
message.delete(); // deletes the message
message.edit('new content'); // edits the message ONLY if it is sent by the bot and returns the SafeMessage object
```

{% hint style="warning" %}
Make sure you're only editing the messages that you create in the scripts, otherwise they will throw an error!
{% endhint %}

Let's do another example:

{% code-tabs %}
{% code-tabs-item title="logging-naughty-word-filter.js" %}
```javascript
if (message.getContents().contains("naughty word")) {
    message.getGuild().getChannelById(id("591735345406672935")).send(
        message.getAuthor().getName() + " said a naughty word!"
    );
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

This example does not delete the message, but instead logs the event in the channel with the ID of `591735345406672935`. As documented earlier, `getGuild()` returns the guild for the message, which allows you to then make your own. The channel `591735345406672935`could be an audit log channel. A new method has been introduced here, called `id`. This method creates a Snowflake object so that you can get channels and the like.

Maybe we want to add some punishments into the mix?

{% code-tabs %}
{% code-tabs-item title="punishing-naughty-word-filter.js" %}
```javascript
if (message.getContents().contains("naughty word")) {
    message.getAuthor().ban("posted a naughty word!", 7);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

This is a punishing version of our naughty word filter. This bans the author of the message for "posted a naughty word!" and then removes the last 7 days of messages.

Let's put everything we've learnt together:

{% code-tabs %}
{% code-tabs-item title="ultimate-naughty-word-filter.js" %}
```javascript
if (message.getContents().contains("naughty word")) {
    message.getGuild().getChannelById(id("591735345406672935")).send(
        message.getAuthor().getName()
        + " sent a naughty word, so I deleted it and banned them!"
    );
    message.getAuthor().ban("said a naughty word!", 0); // let's be forgiving
    message.delete();
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

And here's the final rundown.

* Check the message to see if it has "naughty word" in it.
* Get the guild the message was posted in.
* Get a channel in the guild by ID "591735345406672935".
* Post a message saying "Username sent a naughty word, so I deleted it and banned them!".
* Get the message author and ban them with the reason "said a naughty word!", but we don't delete any messages from them so that their message history isn't lost.
* Delete the message.

Of course you can do much more advanced things, but that is out of the scope of this guide.

