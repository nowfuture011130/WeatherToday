# WeatherToday
A website for predicting city weather, developed using Vue.js, featuring visual effects powered by Tailwind CSS, and utilizing the OpenWeather API to retrieve today's and future weather forecasts. The website has been deployed using Netlify, and the website's address is: https://cool-mochi-f1b5ba.netlify.app/.

#Features
This website consists of the following sections:
1) An "About" button that displays usage instructions. When clicked, it smoothly animates to show the instructions and uses a mask to cover the background window to prevent users from accidentally clicking other buttons while viewing the instructions. The "about" icon is a font image introduced through Tailwind to enhance resource loading speed. The instruction information and the instruction window are separated into different components, making it easier to modify the instruction content in the future.
2) A search bar that utilizes the Google Auto Complete API to provide suggestions for user-input cities and retrieve their corresponding coordinates. The input system employs a lazy-trigger mechanism to conserve data usage, only triggering the API call when the user stops typing for 0.3 seconds. If there is no internet connection or no search results, it will notify the user accordingly.
3) A user interface that displays weather information for a city, fetching data from the WeatherToday API based on the coordinates of the location and displaying corresponding weather images. This interface also allows users to add cities to their browser's cache for easy access in the future.
4) A list of favorite cities, displaying all the city information stored in memory along with the weather for the next two days. Users can click on a city card to access more detailed information.

#How to use

## Project Setup

```sh
npm install
```
### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
