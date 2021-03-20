# Working with OpenWeather API

Before we use OpenWeather's API we would need their API Key for current data.

## Coding in the functionality for the !weather command

1. We add the api key from OpenWeather
    ```
    api_key = "your_token"
    ```

2. We setup the discord command that triggers getting the weather data. Under the **on_message()** function we add another if statement.
    ```
    if message.content.startswith('!weather'):
        location = message.content.replace('!weather', '').lower()
        init_data = get_weather(location)
    ```

## Retrieving Data using the OpenWeather API

1. To retrieve data from OpenWeather API navigate to the website [OpenWeather](https://openweathermap.org/api) and click the API doc button for Current Weather Data.

2. In the list of API Calls by city name. Copy the first API call

3. In the get_weather() function we use this copied API call and just extract the city data.
    ```
    def get_weather(city_name):
        if len(city_name) >= 1:
            url = f'http://api.openweathermap.org/data/2.5/weather?q={city_name}&appid={api_key}&units=imperial'
            data = json.loads(requests.get(url).content)  # Getting the Data from the URL
            pprint(data)
        return data
    ```

4. At this point if we run our bot and type in "!weather" followed by the city name we will get a console output that has data of the region from wind_speed to the locations coordinates.

5. We need to only keep the temperature info and have to now display that output in our discord server