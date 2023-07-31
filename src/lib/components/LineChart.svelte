<script lang="ts">
	import Chart from 'chart.js/auto';
	import { onMount } from 'svelte';

	let chartValues = [[1, 2]];
	let chartLabels = ['January', 'February', 'March', 'April', 'May', 'June', 'July'];
	let chartCanvas: HTMLCanvasElement;

	onMount(async () => {
		new Chart(chartCanvas, {
			type: 'line',
			data: {
				labels: chartLabels,
				datasets: [
					{
						label: 'Revenue',
						data: chartValues,
						backgroundColor: 'rgb(255, 99, 132)',
						borderColor: 'rgb(255, 99, 132)'
					}
				]
			}
			// options: {
			// 	plugins: {
			// 		legend: {
			// 			labels: {
			// 				color: 'blue'
			// 			}
			// 		}
			// 	},
			// 	scales: {
			// 		x: {
			// 			grid: {
			// 				color: 'blue'
			// 			}
			// 		}
			// 	}
			// }
		});
	});
</script>

<div class="container bg-surface-100-800-token p-6 rounded-lg">
	<canvas bind:this={chartCanvas} id="myChart" />
</div>
