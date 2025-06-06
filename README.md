# Weather Forecast Prediction

This repository hosts a web application built with Django that provides current weather information and a 5-hour forecast for any specified city. It leverages the OpenWeatherMap API for real-time data and employs machine learning models (Random Forest Classifier and Regressor) for predicting future temperature and humidity, as well as the likelihood of rain.

## Features

  * **Current Weather Display:** Get up-to-date weather details including temperature, "feels like" temperature, min/max temperatures, humidity, pressure, wind speed, visibility, and cloud cover.
  * **5-Hour Forecast:** Predicts temperature and humidity for the next 5 hours based on historical data and machine learning models.
  * **Rain Prediction:** Estimates the probability of rain using a trained classification model.
  * **Interactive UI:** A user-friendly interface to search for cities and view weather insights.
  * **Dynamic Backgrounds:** The background of the application changes based on the current weather description.
  * **Temperature Trend Chart:** Visualizes the predicted temperature trend for the next 5 hours using Chart.js.

## Technologies Used

  * **Backend:**
      * Django (Python web framework)
      * `requests`: For making API calls to OpenWeatherMap.
      * `pandas`: For data manipulation and analysis of historical weather data.
      * `numpy`: For numerical operations, especially in preparing data for models.
      * `scikit-learn`: For implementing machine learning models (`RandomForestClassifier`, `RandomForestRegressor`, `LabelEncoder`).
      * `datetime`, `pytz`: For handling date and time, and timezone conversions.
  * **Frontend:**
      * HTML
      * CSS (with Google Fonts 'Poppins' and Bootstrap Icons)
      * JavaScript (for Chart.js integration)
      * `Chart.js`: For creating interactive charts.
  * **API:**
      * OpenWeatherMap API

## Setup and Installation

Follow these steps to set up and run the project locally:

### 1\. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/Weather-Forecast-Prediction.git
cd Weather-Forecast-Prediction
```

### 2\. Create a Virtual Environment (Recommended)

```bash
python -m venv venv
source venv/bin/activate  # On Windows: `venv\Scripts\activate`
```

### 3\. Install Dependencies

```bash
pip install -r requirements.txt
```

*(Create a `requirements.txt` file if you don't have one, by running `pip freeze > requirements.txt` after installing all necessary libraries)*

### 4\. Obtain OpenWeatherMap API Key

1.  Go to the [OpenWeatherMap website](https://openweathermap.org/api).
2.  Sign up for a free account.
3.  Generate an API key.
4.  Replace `'YOUR_API_KEY'` in `views.py` with your actual API key:
    ```python
    API_KEY = 'YOUR_API_KEY' # Replace with your OpenWeatherMap API key
    ```

### 5\. Prepare Historical Data

This application relies on a `weather.csv` file for training the machine learning models.

  * Place your `weather.csv` file in a location accessible by your Django project.
  * Update the `csv_path` variable in `views.py` to point to the correct path of your `weather.csv` file:
    ```python
    csv_path = os.path.join('C:\\Users\\ASUS\\OneDrive\\Desktop\\Weather_pred\\weather.csv') # Update this path
    ```
    **Note:** It's highly recommended to make this path dynamic or relative for better portability across different environments. For example, you could place `weather.csv` inside your Django app's directory (e.g., `static/data/weather.csv`) and adjust the path accordingly.

### 6\. Run Django Migrations

```bash
python manage.py makemigrations
python manage.py migrate
```

### 7\. Run the Development Server

```bash
python manage.py runserver
```

The application should now be accessible at `http://127.0.0.1:8000/weather/` (or similar, depending on your `urls.py` configuration).

## Project Structure

  * **`views.py`**: Contains the core logic for fetching current weather data, reading historical data, preparing data for machine learning, training the models (Random Forest Classifier for rain, Random Forest Regressor for temperature and humidity), and rendering the `weather.html` template with the gathered information and predictions.
  * **`weather.html`**: The main HTML template responsible for the user interface, displaying current weather, forecast, and integrating the Chart.js for temperature trends.
  * **`manage.py`**: Django's command-line utility for administrative tasks.
  * **`chartSetup.js`**: JavaScript file responsible for setting up and rendering the Chart.js graph for temperature trends.
  * **`style.css`**: Provides the styling for the web application, including dynamic background images based on weather conditions.
  * **`static/`**: Directory for static files (CSS, JS, images).
      * `img/`: Contains images used for dynamic backgrounds and icons.
      * `css/`: Contains `style.css`.
      * `js/`: Contains `chartSetup.js`.
  * **`WeatherPredict/`**: (Likely your Django project's main configuration directory)
      * `settings.py`: Project settings.
      * `urls.py`: URL routing for the project.

## How it Works

1.  **User Input:** The user enters a city name into the search bar on the `weather.html` page.
2.  **Current Weather Fetching:** The `weather_view` function in `views.py` receives the city name. It then calls `get_current_weather` which uses the OpenWeatherMap API to retrieve current weather conditions for that city.
3.  **Historical Data Processing:** The `read_historical_data` function loads a `weather.csv` file. This data is then preprocessed by `prepare_data` which uses `LabelEncoder` to convert categorical 'WindGustDir' and 'RainTomorrow' into numerical representations.
4.  **Machine Learning Models:**
      * **Rain Prediction:** A `RandomForestClassifier` is trained on the historical data to predict if it will rain (`RainTomorrow`). The current weather's wind direction is encoded and used as an input for this prediction.
      * **Temperature and Humidity Prediction:** `RandomForestRegressor` models are trained for both temperature and humidity using `prepare_regression_data`. These models then predict the next 5 hours of temperature and humidity based on the current values.
5.  **Data Rendering:** All the retrieved current weather data and the future predictions are passed to the `weather.html` template as context.
6.  **Frontend Display:** `weather.html` dynamically displays the weather information. `chartSetup.js` then takes the predicted hourly temperatures and visualizes them using Chart.js.

## Contributing

Contributions are welcome\! If you'd like to improve this project, please feel free to:

1.  Fork the repository.
2.  Create a new branch (`git checkout -b feature/your-feature-name`).
3.  Make your changes.
4.  Commit your changes (`git commit -m 'Add some feature'`).
5.  Push to the branch (`git push origin feature/your-feature-name`).
6.  Open a Pull Request.
