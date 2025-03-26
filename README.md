# OpenWeatherMap
The Weather App is a simple Python-based application that fetches and displays real-time weather data for any city using the OpenWeatherMap API. This project is perfect for beginners looking to learn how to work with APIs and handle JSON data in Python.
import requests

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GitHub README Display</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
        }
        .markdown-body {
            max-width: 800px;
            margin: auto;
        }
    </style>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/github-markdown-css/4.0.0/github-markdown.min.css">
</head>
<body>
    <div id="content" class="markdown-body"></div>
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <script>
        async function fetchReadme() {
            const response = await fetch('https://raw.githubusercontent.com/anurudhrasundaram/anurudhrasundaram/main/README.md');
            const text = await response.text();
            document.getElementById('content').innerHTML = marked(text);
        }

        fetchReadme();
    </script>
</body>
</html>

def get_weather(city_name, api_key):
    base_url = "https://api.openweathermap.org/data/2.5/weather"
    params = {
        "q": city_name,
        "appid": api_key,
        "units": "metric"  # Use 'imperial' for Fahrenheit
    }

    try:
        response = requests.get(base_url, params=params)
        response.raise_for_status()  # Raise an error for HTTP codes >= 400
        data = response.json()

        # Extract weather details
        weather = {
            "City": data["name"],
            "Temperature": data["main"]["temp"],
            "Description": data["weather"][0]["description"],
            "Humidity": data["main"]["humidity"],
            "Wind Speed": data["wind"]["speed"]
        }
        return weather

    except requests.exceptions.RequestException as e:
        return f"Error: {e}"
    except KeyError:
        return "Invalid city name or API response error."

def main():
    print("Weather App")
    api_key = input("Enter your OpenWeatherMap API key: ")
    while True:
        city = input("\nEnter a city name (or type 'exit' to quit): ")
        if city.lower() == "exit":
            print("Exiting the Weather App. Goodbye!")
            break

        weather = get_weather(city, api_key)
        if isinstance(weather, dict):
            print("\nWeather Details:")
            for key, value in weather.items():
                print(f"{key}: {value}")
        else:
            print(weather)

if __name__ == "__main__":
    main()
