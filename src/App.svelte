<script lang="ts">
	import { LatLng, TileLayer } from "leaflet";
	import AnswerMap from "./AnswerMap.svelte";
</script>

<main>
	<AnswerMap
		mapLayer={new TileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
			attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
			maxZoom: 19,
		})}
		from={new LatLng(57.616227, 11.775967)}
		targets={[
			new LatLng(57.618611, 11.768512),
			new LatLng(57.609116, 11.784028),
			new LatLng(57.61226, 11.790719),
			new LatLng(57.616056, 11.794048),
		]}
	/>
</main>

<style lang="scss">
	main {
		width: 100%;
		height: 100%;
	}
</style>
