# Getting the Bot Up and Running

## Adding the Initial Code

1. Create a new python file:

2. Import discord, requests and json
    ```
    import discord
    import requests
    import json
    from pprint import pprint
    ```

3. Connect to the discord client and use the Token from the Discord Developer Account to get the bot up and running
    ```
    client = discord.Client()
    client.run("Your_Token")
    ```

4. Now we will go and add the on_ready() method. This method notifies us when the discord bot becomes online. Discord.py is an asynchronous library so we use async and await in our function.
    ```
    @client.event
    async def on_ready():
        await client.change_presence(
            activity=discord.Activity(type=discord.ActivityType.listening, name='Providing Weather'))
        print("Weather Bot has logged in as {0.user}".format(client))
    ```

5. Let's Run our Program Now:

