# Wrapping up the Discord Bot

## Parsing out Extraneous Information

1. Looking at the Output on the console we notice that our useful information lies under the **'main'** key of our JSON output. We are going to restrict our information only to that field then. Even in the main key we do not need humidity and pressure so we can go ahead and delete those.

2. We add another function called parse_weather_data() 
    ```
    def parse_weather_data(data):
        data = data['main']
        del data['humidity']
        del data['pressure']
        return data
    ```

## Displaying the Message on Discord

1. Currently the info comes in as temp, feels_like, temp_min and temp_max but I want to change the way the name outputs so we create a dictionary called name_change that takes in the original name as key and the name we want to display as value
    ```
    name_change = {
        'temp': 'Current Temperature',
        'feels_like': 'Feels Like',
        'temp_min': 'Minimum Temperature',
        'temp_max': 'Maximum Temperature'
    }
    ```

2. We are going to create another function **weather_message()** that returns a discord message displaying the data we have got from the **parse_weather_data()** function.
    ```
    def weather_message(data, location):
        location = location.title()
        message = discord.Embed(
            title=f'{location} Weather',
            description=f'Weather For {location}.',
            color=color_scheme
        ) # Information that will be sent on discord
        for key in data:
            message.add_field(name=name_change[key], value=str(data[key]), inline=False)
        return message
    ```

3. Now that we have everything setup let us call these functions back in the **on_message()** function

4. Pass the initial data to the **parse_weather_data()** function and then send the message to the user on the discord server. Our final if statement in the **on_message()** function should look like this.
    ```
    if message.content.startswith('!weather'):
        location = message.content.replace('!weather', '').lower()
        init_data = get_weather(location)
        data = parse_weather_data(init_data)
        await message.channel.send(embed=weather_message(data, location))
    ```

5. This is the final code for the **on_message()** function
    ```
    @client.event
    async def on_message(message):
        if message.author == client.user:
            return
        if message.content.startswith('!hello'):
            await message.channel.send("Hello World")
        if message.content.startswith('!apicall'):
            temp = get_weather("Dallas")  # Has to be Replaced with the String extracted from User Input
            await message.channel.send("The Temperature in this city is {}".format(temp))
        if message.content.startswith('!weather'):
            location = message.content.replace('!weather', '').lower()
            init_data = get_weather(location)
            data = parse_weather_data(init_data)
            await message.channel.send(embed=weather_message(data, location))
    ```

    6. When we run our Code and type in !weather Dallas we should get an output with current weather data.
    
    7. Congratulations!!!🎊. The discord bot is now running like it should but it is not running 24/7 only when we are running the program from our terminal. 