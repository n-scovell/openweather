🌦️ OpenWeather App

A simple weather application that fetches real-time weather data using the OpenWeather API.

This project demonstrates how to work with external APIs, handle user input, and dynamically display weather data in a clean UI.

🚀 Live Demo

(Add your deployed link here if you have one)

📦 Features
🌍 Search weather by city
🌡️ Displays temperature and conditions
☁️ Uses real-time data from OpenWeather
⚡ Lightweight and fast
🧠 Simple, easy-to-understand codebase
🔑 API Key Required

This project uses the OpenWeather API, which requires a free API key.

To run this project, you must get your own key:

👉 Get an API key from OpenWeather

How to get your API key:
Create an account on OpenWeather
Go to your dashboard
Navigate to "My API Keys"
Generate or copy your API key

Once created, your key will be a long string used to authenticate your requests

⚙️ Setup Instructions
Clone the repository
git clone https://github.com/n-scovell/openweather.git
cd openweather
Add your API key

Find where the API key is used in the code (likely in your JavaScript file) and replace it:

const API_KEY = "your_api_key_here";
Run the project

Just open index.html in your browser
(or use a live server if you prefer)

🧠 How It Works
The app sends a request to the OpenWeather API
The API returns JSON weather data
JavaScript parses the response
The UI updates dynamically with the results
⚠️ Notes
API keys may take a few minutes to activate after creation
Free tier has request limits
Do not expose your API key in production apps (use a backend instead)
🛠️ Tech Stack
HTML
CSS
JavaScript (Vanilla)
OpenWeather API
📁 Project Structure
/openweather
  ├── index.html
  ├── style.css
  └── script.js
🙌 Acknowledgements
Weather data provided by OpenWeather
📜 License

This project is open source and available under the MIT License.
