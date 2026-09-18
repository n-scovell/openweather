# OpenWeather App

A lightweight weather application built with vanilla JavaScript that retrieves real-time weather data from the OpenWeather API and dynamically updates the interface based on a user's city search.

## Features

* Search weather by city
* Retrieve current weather data from the OpenWeather API
* Display temperature and weather conditions
* Handle user input and API responses
* Dynamically update the UI

## Tech Stack

* HTML5
* CSS3
* Vanilla JavaScript
* OpenWeather API

## How It Works

The application sends a request to the OpenWeather API using the user's city search. The returned JSON data is parsed and used to update the weather information displayed in the interface.

## Running Locally

Clone the repository:

```bash
git clone https://github.com/n-scovell/openweather.git
cd openweather
```

The application requires an OpenWeather API key.

Configure the key according to the application's JavaScript configuration, then open the application in a browser or run it using a local development server.

**Important:** Never commit a real API key to a public repository.

## Project Purpose

This project demonstrates working with a third-party REST API, asynchronous JavaScript, JSON data, user input, and dynamic DOM updates without relying on a JavaScript framework.

## License

MIT
