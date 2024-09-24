# weatherApp

Outline: 

  Main page: 

      Components: 

          -Header: App title, location...etc) 
          -Weather info component( temp display, conditions....etc) 
          -Background components: changes color and image changes based on weather


        Components needed: 
          -Weather Fetcher: Fetch the data from weather API
          Ex: WE will use useEffect to fetch the data when components load 

         
  
   
   useEffect(() => {
  fetch(`https://api.openweathermap.org/data/2.5/weather?q=London&appid=YOUR_API_KEY`)
    .then(response => response.json())
    .then(data => {
      // Set weather data to state
      setWeather(data);
    });
}, []);


Example directory:
/public
  index.html          # Main HTML file
  /assets
    /icons            # Weather icons (sunny, cloudy, etc.)
    /images           # Other images (backgrounds, logos, etc.)

/src
  /components
    Header.js         # Header component
    Footer.js         # Footer component
    WeatherInfo.js    # Component to display weather info
    WeatherBackground.js # Component for the dynamic background
  /pages
    HomePage.js       # Main landing page
    WeatherPage.js    # Page where weather details are displayed
  /hooks
    useWeatherApi.js  # Custom hook to fetch weather data from API
  /styles
    main.css          # General styles
    WeatherBackground.css # Styles specific to the dynamic background
    WeatherInfo.css   # Styles for weather data display
  App.js              # Main app component
  index.js            # Entry point for React app


Example App.js: 

import React, { useState, useEffect } from 'react';
import Header from './components/Header';
import WeatherInfo from './components/WeatherInfo';
import WeatherBackground from './components/WeatherBackground';
import useWeatherApi from './hooks/useWeatherApi';
import './styles/main.css';

function App() {
  // State to store weather data and condition
  const [weatherData, setWeatherData] = useState(null);
  const [loading, setLoading] = useState(true);

  // Fetch weather data using the custom hook
  const weatherApiResponse = useWeatherApi(); // Custom hook that fetches the weather API
  
  useEffect(() => {
    if (weatherApiResponse) {
      setWeatherData(weatherApiResponse);
      setLoading(false);
    }
  }, [weatherApiResponse]);

  if (loading) {
    return <div>Loading...</div>;
  }

  return (
    <div className="App">
      {/* Dynamic background based on weather */}
      <WeatherBackground condition={weatherData.condition} />
      
      {/* Header of the app */}
      <Header title="Weather App" />
      
      {/* Main weather info */}
      <WeatherInfo 
        temperature={weatherData.temperature}
        condition={weatherData.condition}
        location={weatherData.location}
      />
    </div>
  );
}

export default App;



  
