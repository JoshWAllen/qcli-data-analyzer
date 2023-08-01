<script lang="ts">
	import Chart from 'chart.js/auto';
	import { onMount } from 'svelte';

	export let chartValues: number[][];
	export let chartLabels: string[];

	let chartCanvas: HTMLCanvasElement;

	interface Dataset {
		label: string;
		data: number[];
	}

	let datasets: Dataset[] = [
		{ label: 'chart1', data: [1, 2, 3] },
		{ label: 'chart1', data: [5, 6, 7] }
	];
	$: {
		datasets = chartValues.map((dataset, i) => {
			return { label: `Chart ${i}`, data: dataset };
		});
	}

	let chartInstance: any;

	$: if (chartInstance) {
		chartInstance.data.datasets = datasets;
		chartInstance.update();
	}

	onMount(async () => {
		chartInstance = new Chart(chartCanvas, {
			type: 'line',
			data: {
				labels: chartLabels,
				datasets: datasets
			},
			options: {
				maintainAspectRatio: false
				// plugins: {
				// 	legend: {
				// 		labels: {
				// 			color: 'blue'
				// 		}
				// 	}
				// },
				// scales: {
				// 	x: {
				// 		grid: {
				// 			color: 'blue'
				// 		}
				// 	}
				// }
			}
		});
	});
</script>

<div class="w-full h-96 relative bg-surface-100-800-token p-6 rounded-lg">
	<canvas bind:this={chartCanvas} id="myChart" />
</div>
