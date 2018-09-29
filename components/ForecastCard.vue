<template>
	<v-flex xs12 sm4>
		<v-card tile>
			<p>{{forecast.temperatureMin}}</p>
		</v-card>
	</v-flex>
</template>

<script>
	export default {
		props: ['forecast']
	}
</script>

<style></style>