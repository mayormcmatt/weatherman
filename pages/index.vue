<template>
	<v-container fluid>
		<v-layout row wrap justify-center align-center>
			<v-flex xs2 md12>
				<div class="text-xs-center">
					<input type="text" name="address" id="address-input" placeholder="Please enter a ZIP Code and hit Enter"
					 v-model.lazy="addressInput" v-on:keyup.enter="weatherLookup">

					<p>{{addressInput}}</p>

					<div><span>{{lat}}</span><span>{{lng}}</span></div>

					<div v-if="weatherResponse.daily">
						<v-container fluid grid-list-md text-xs-center>
							<v-layout row wrap>
								<forecast-card v-for="day in weatherResponse.daily.data" :forecast="day" :key="day.time"></forecast-card>
							</v-layout>
						</v-container>
					</div>
				</div>
			</v-flex>
		</v-layout>
	</v-container>
</template>

<script>
	import ForecastCard from '~/components/ForecastCard.vue'

	export default {
		components: {
			ForecastCard
		},
		data() {
			return {
				addressInput: '',
				key: '37ba52688d5a1c4d2fa29d4099ec7c3f',
				lat: '',
				lng: '',
				weatherResponse: {}
			}
		},
		methods: {
			weatherLookup: function () {
				const url = 'http://www.mapquestapi.com/geocoding/v1/address?key=YwK801yhAE9SXv7XZa7hWc9d3aJlVrYU&location=' +
					encodeURIComponent(this.addressInput)
				this.$axios.$get(url).then((response) => {
					this.lat = response.results[0].locations[0].latLng.lat
					this.lng = response.results[0].locations[0].latLng.lng
					const weatherUrl =
						`https://cors-anywhere.herokuapp.com/https://api.darksky.net/forecast/${this.key}/${this.lat},${this.lng}?exclude=minutely,hourly,flags`
					return this.$axios.$get(weatherUrl, {
						headers: {
							'crossDomain': true,
							'Access-Control-Allow-Origin': '*',
							'Content-Type': 'application/json'
						}
					})
				}).then((response) => {
					this.weatherResponse = response
				}).catch((e) => {
					console.log(e)
				})
			}
		}
	}
</script>