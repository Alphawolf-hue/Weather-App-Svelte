<script>
	import { Router, Link, Route } from "svelte-routing";
	import {onMount} from 'svelte';
	import Chart from "./components/Chart.svelte";
	import Weather from "./components/Weather.svelte";
	import Navbar from "./layout/Navbar.svelte";

   let city = 'Bhopal';
   let data = {}
   var description;
   var windSpeed;
   var humidity;
   var temperature;
   var icon;

   onMount(() => {
	fetchWeather(city).then(res=>{
		  displayWeather(res); 
	   });
   })

   const onSubmit = (event) => {
	   event.preventDefault
	   fetchWeather(city).then(res=>{
		   displayWeather(res);
	   });
   }
   
   function displayWeather(res){
		   data = res;
		   description = data.weather[0].description;
		   temperature = data.main.temp;
		   windSpeed = data.wind.speed;
		   humidity = data.main.humidity;
		   icon = data.weather[0].icon
		   document.getElementById("icon").src = "https://openweathermap.org/img/wn/" + icon + ".png";
   }
   
   function fetchWeather (city) {
   return fetch(
	 "https://api.openweathermap.org/data/2.5/weather?q=" +
	   city +
	   "&units=metric&appid=8d8247022223ca9e1d9a07e3f7590757"
   )
   .then((response) => {
	   if (!response.ok) {
		 alert("No weather found.");
		 throw new Error("No weather found.");
	   }
	   return response;
   }).then(res=>res.json())


	
 }
</script>

<main class="uk-height-large  uk-padding-remove-top" >
	<h1>SVELTE-WEATHER</h1>
   <Router>
	   <Navbar/>
   </Router>
   <div class="uk-padding-small">
   <div class="uk-section uk-section-xlarge uk-section-muted uk-padding-large">
	   <div class="uk-flex">
		   <form on:submit={onSubmit}>
			   <div class="uk-grid-column-small uk-grid-row-large uk-padding uk-padding-remove-top" uk-grid>
				   <input bind:value={city} type="text" id="city" placeholder="Enter city" />
				   <button class="uk-button uk-button-default" type="submit"><span uk-icon="icon: search"></span></button>
				   <h3>Weather in {city} </h3>
			   </div>
		   </form>	
	   </div>
	   <div class="uk-container" background-color="blue">
		   <div class="uk-grid-match uk-child-width-expand@m" uk-grid>
			   <div class="uk-card uk-card-default uk-card-body uk-margin-right uk-width-1-3@m uk-padding-remove ">
				   <Weather
				   description={description}
				   temperature={temperature} 
				   humidity={humidity}
				   windSpeed={windSpeed}
				   />
			   </div>
			   <div class="uk-card uk-card-default uk-margin-left uk-card-body uk-padding-remove">
				   <div class="uk-width-1-1@m uk-padding  uk-padding-remove-bottom">
					   <div class="uk-card uk-card-default uk-card-body">
						   
					   </div>
				   </div>
				   <div>
					   <div class="uk-child-width-1-3@m uk-grid-small uk-grid-match uk-padding" uk-grid>
						   <div>
							   <div class="uk-card uk-card-small uk-card-primary uk-card-body">
								   <div class="uk-card-title">today</div>
								   <div class="uk-card-body">humidity</div >
							   </div>
						   </div>
						   <div>
							   <div class="uk-card uk-card-small uk-card-default uk-card-body">
								   <div class="uk-card-title">tomorrow</div>
								   <div class="uk-card-body">humidity</div>
							   </div>
						   </div>
						   <div>
							   <div class="uk-card uk-card-small uk-card-default uk-card-body">
								   <div class="uk-card-title">next 2 days</div>
								   <div class="uk-card-body">humidity</div>
							   </div>
						   </div>
					   </div>
				   </div>
			   </div>
		   </div>
	   </div>
   </div>
</div>
</main>

<style>
   main {
	   text-align: center;
	   padding: 1em;
	   max-width: 150px;
	   margin: 0 auto;
	   background-color: rgb(111, 206, 238);
   }
   h1 {
  font-size: 60px;
  font-family: "Garamond",cursive;
  font-weight: bold;
  color: darkred;
  line-height: 1.1;
  margin: 10 px 0;
  margin-bottom: 1rem;
  letter-spacing: 0.1rem;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  text-transform: uppercase;
  display: flex;
  align-items: center;
  padding: 10px;
  justify-content: center;
}

@media screen and (max-width: 767px) {
  h1 {
    font-size: 3rem;
  }
}

@media screen and (max-width: 575px) {
  h1 {
    font-size: 2rem;
  }
}

/* Just for demo purposes */
body {
  font-family: 'Lato', sans-serif;
  color: #555;
  background-color: #e9e9e9;
}

.container {
  max-width: 680px;
  margin: 0 auto;
  padding: 2rem;
}
   @media (min-width: 640px) {
	   main {
		   max-width: none;
	   }
   }


</style>