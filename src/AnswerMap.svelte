<script lang="ts">
	import "leaflet/dist/leaflet.css";
	import { Map, LatLngBounds, LatLng, TileLayer, Tooltip, Point } from "leaflet";
	import { GeodesicLine } from "leaflet.geodesic";

	export let from: LatLng;
	export let targets: ReadonlyArray<LatLng>;
	export let mapLayer: TileLayer;
	export let fromIcon: string = "🔷";
	export let targetIcon: string = "🔶";
	export let goalIcon: string = "⭐";

	function LoadMap(node: HTMLElement): void {
		const map = new Map(node, {
			zoomControl: false,
		});
		map.addLayer(mapLayer);

		const bound = new LatLngBounds([from, ...targets]);
		map.fitBounds(bound);

		for (const target of targets) {
			addTarget(map, from, target);
		}

		addPoint(map, from, fromIcon);
	}

	function addTarget(map: Map, from: LatLng, target: LatLng): void {
		addLine(map, from, target);
		addPoint(map, target, targetIcon);
		addDistance(map, from, target);
	}

	function addPoint(map: Map, point: LatLng, icon: string): void {
		map.addLayer(
			new Tooltip(point, {
				content: icon,
				permanent: true,
				direction: "center",
				opacity: 1,
				className: "target-icon",
			}),
		);
	}

	function addLine(map: Map, from: LatLng, target: LatLng): void {
		map.addLayer(
			new GeodesicLine([from, target], {
				weight: 2,
				steps: 8,
				wrap: true,
			}),
		);
	}

	function addDistance(map: Map, from: LatLng, target: LatLng): void {
		const distance = map.distance(from, target);
		map.addLayer(
			new Tooltip(target, {
				content: humanize(distance),
				permanent: true,
				direction: "center",
				opacity: 1,
				className: "distance-label",
				offset: new Point(0, 30),
			}),
		);
	}

	function humanize(length: number): string {
		if (length >= 10_000) {
			return `${Math.round(length / 1_000)} km`;
		}
		return `${Math.round(length)} m`;
	}
</script>

<div use:LoadMap />

<style lang="scss">
	div {
		width: 100%;
		height: 100%;
	}

	:global(.target-icon) {
		background: none;
		border: none;
		box-shadow: none;
		font-size: 1.5rem;
	}
	:global(.distance-label) {
		background-color: rgba(255, 255, 255, 0.75);
		padding: 3px 6px;
	}
</style>
