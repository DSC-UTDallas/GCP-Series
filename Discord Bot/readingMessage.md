# Replying to User Commands

Before we go ahead and use OpenWeather's API let us do a Quick Hello World in Discord.py

1. The function in the discord.py library that allows listening to commands is **on_message()**:

2. The following code goes ahead and adds code to reply to the user when they type in !hello in discord
    ```
    @client.event
    async def on_message(message):
        if message.author == client.user:
            return
        if message.content.startswith(!hello):
            await message.channel.send("Hello World")
    ```

3. When we start the program and the bot starts up if we type in !hello. The bot returns us with "Hello World"
