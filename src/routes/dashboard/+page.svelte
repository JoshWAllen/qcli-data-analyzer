<script lang="ts">
	import LineChart from '$lib/components/LineChart.svelte';
	import {
		FileDropzone,
		Table,
		tableMapperValues,
		Paginator,
		ListBox,
		ListBoxItem
	} from '@skeletonlabs/skeleton';
	import { parse } from 'papaparse';

	let files: FileList;

	function onChangeHandler(e: any): void {
		if (!e.target.files) return;
		const file = e.target.files[0];
		parse(file, {
			download: true,
			header: true,
			skipEmptyLines: true,
			complete: (results: any) => {
				const data = results.data;
				sourceData = data;
			}
		});
	}
	interface CSVRowObject {
		[key: string]: any;
	}

	let sourceData: CSVRowObject[] = [
		{ position: 1, name: 'Hydrogen', weight: 1.0079, symbol: 'H' },
		{ position: 2, name: 'Helium', weight: 4.0026, symbol: 'He' },
		{ position: 3, name: 'Lithium', weight: 6.941, symbol: 'Li' },
		{ position: 4, name: 'Beryllium', weight: 9.0122, symbol: 'Be' },
		{ position: 5, name: 'Boron', weight: 10.811, symbol: 'B' }
	];

	let headers = Object.keys(sourceData[0]);

	// PaginatorSettings
	let page = {
		offset: 0,
		limit: 5,
		size: 0,
		amounts: [1, 2, 5, 10, 25, 50]
	};

	function updateColumns() {
		headers = Object.keys(sourceData[0]);
	}

	function updatePage() {
		page = {
			...page,
			size: sourceData.length
		};
	}

	$: if (sourceData.length > 0) {
		updatePage();
		updateColumns();
	}

	$: paginatedSource = sourceData.slice(
		page.offset * page.limit, // start
		page.offset * page.limit + page.limit // end
	);

	// Recalculate table data when sourceData changes
	$: tableSimple = {
		// A list of heading labels.
		head: headers,
		// The data visibly shown in your table body UI.
		body: tableMapperValues(paginatedSource, headers)
	};

	$: if (files) {
		console.log(files[0].name);
	}

	// $: console.log('headers changed: ', headers);

	const chart1 = [
		[1, 5, 6, 2, 3, 7, 1],
		[2, 3, 4, 1, 2, 4, 6],
		[7, 1, 4, 2, 6, 7, 2],
		[1, 5, 6, 2, 0, 9, 1],
		[2, 5, 4, 1, 2, 8, 6],
		[7, 1, 4, 4, 1, 2, 2]
	];
	const chartLabels = ['January', 'February', 'March', 'April', 'May', 'June', 'July'];

	let numbers = [
		[1, 2],
		[5, 6]
	];

	function addNumber() {
		numbers = [...numbers, [numbers.length + 1, numbers.length + 3, numbers.length + 4]];
	}
</script>

<div class="container h-full mx-auto flex justify-center items-center">
	<div class="w-4/5 space-y-5">
		{#if files}
			<h1 class="h1">{files[0].name}</h1>
		{:else}
			<h1 class="h1">Dashboard</h1>
		{/if}

		<FileDropzone name="files" accept=".csv" bind:files on:change={onChangeHandler}>
			<svelte:fragment slot="lead">(icon)</svelte:fragment>
			<svelte:fragment slot="message">Upload a file or drag and drop</svelte:fragment>
			<svelte:fragment slot="meta">only CSV allowed</svelte:fragment>
		</FileDropzone>
		<button on:click={addNumber} class="btn variant-filled">Add Numbers</button>

		<LineChart chartValues={numbers} {chartLabels} />
		<ListBox multiple>
			{#each Object.keys(sourceData[0]) as header (header)}
				<ListBoxItem bind:group={headers} name="headers" value={header}>{header}</ListBoxItem>
			{/each}
		</ListBox>

		<Table source={tableSimple} />
		{#if headers.length !== 0}
			<Paginator bind:settings={page} showFirstLastButtons={true} showPreviousNextButtons={true} />
		{/if}
	</div>
</div>
