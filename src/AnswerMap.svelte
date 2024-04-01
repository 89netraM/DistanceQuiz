<script lang="ts">
	import "leaflet/dist/leaflet.css";
	import { onDestroy } from "svelte";
	import { Map, LatLngBounds, LatLng, TileLayer, Tooltip, Point } from "leaflet";
	import { GeodesicLine } from "leaflet.geodesic";

	const lineZIndex = 10;
	const iconZIndex = 20;
	const labelZIndex = 30;

	export let from: LatLng;
	export let targets: ReadonlyArray<LatLng>;
	export let mapLayer: TileLayer;
	export let fromIcon: string = "🔷";
	export let targetIcon: string = "🔶";
	export let goalIcon: string = "⭐";

	let map: Map;

	function LoadMap(node: HTMLElement): void {
		map = new Map(node, {
			zoomControl: false,
		});
		map.addLayer(mapLayer);

		const bound = new LatLngBounds([from, ...targets]);
		map.fitBounds(bound);

		addPoint(map, from, fromIcon);

		const farthest = targets.reduce((a, t) => Math.max(a, map.distance(from, t)), 0);
		for (const target of targets) {
			addTarget(map, from, target, farthest);
		}
	}

	function addTarget(map: Map, from: LatLng, target: LatLng, farthest: number): void {
		const distance = map.distance(from, target);
		addLine(map, from, target);
		addPoint(map, target, distance === farthest ? goalIcon : targetIcon);
		addDistance(map, target, distance);
	}

	function addPoint(map: Map, point: LatLng, icon: string): void {
		map.addLayer(
			new Tooltip(point, {
				content: icon,
				permanent: true,
				direction: "center",
				opacity: 1,
				className: "target-icon",
				zIndexOffset: iconZIndex,
			}),
		);
	}

	function addLine(map: Map, from: LatLng, target: LatLng): void {
		map.addLayer(
			new GeodesicLine([from, target], {
				weight: 2,
				steps: 8,
				wrap: true,
				zIndexOffset: lineZIndex,
			}),
		);
	}

	function addDistance(map: Map, target: LatLng, distance: number): void {
		map.addLayer(
			new Tooltip(target, {
				content: humanize(distance),
				permanent: true,
				direction: "center",
				opacity: 1,
				className: "distance-label",
				offset: new Point(0, 30),
				zIndexOffset: labelZIndex,
			}),
		);
	}

	function humanize(length: number): string {
		if (length >= 10_000) {
			return `${Math.round(length / 1_000)} km`;
		}
		return `${Math.round(length)} m`;
	}

	onDestroy(() => {
		map.remove();
	});
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
