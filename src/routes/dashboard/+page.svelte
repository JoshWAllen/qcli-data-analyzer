<script lang="ts">
	import { FileDropzone, Table, tableMapperValues, type TableSource } from '@skeletonlabs/skeleton';
	import { parse } from 'papaparse';
	let files: FileList;

	function onChangeHandler(e: any): void {
		if (!e.target.files) return;
		let file = e.target.files[0];
		parse(file, {
			download: true,
			header: true,
			skipEmptyLines: true,
			complete: (results) => {
				console.log(results.data);
			}
		});
	}

	const sourceData = [
		{ position: 1, name: 'Hydrogen', weight: 1.0079, symbol: 'H' },
		{ position: 2, name: 'Helium', weight: 4.0026, symbol: 'He' },
		{ position: 3, name: 'Lithium', weight: 6.941, symbol: 'Li' },
		{ position: 4, name: 'Beryllium', weight: 9.0122, symbol: 'Be' },
		{ position: 5, name: 'Boron', weight: 10.811, symbol: 'B' }
	];
	const tableSimple: TableSource = {
		// A list of heading labels.
		head: ['Name', 'Symbol', 'Weight'],
		// The data visibly shown in your table body UI.
		body: tableMapperValues(sourceData, ['name', 'symbol', 'weight']),
		// Optional: The data returned when interactive is enabled and a row is clicked.
		meta: tableMapperValues(sourceData, ['position', 'name', 'symbol', 'weight']),
		// Optional: A list of footer labels.
		foot: ['Total', '', '<code class="code">5 Rows</code>']
	};
</script>

<div class="container h-full mx-auto flex justify-center items-center">
	<div class="w-2/3 space-y-5">
		<h1 class="h1">Dashboard</h1>
		<FileDropzone name="files" accept=".csv" bind:files on:change={onChangeHandler}>
			<svelte:fragment slot="lead">(icon)</svelte:fragment>
			<svelte:fragment slot="message">Upload a file or drag and drop</svelte:fragment>
			<svelte:fragment slot="meta">only CSV allowed</svelte:fragment>
		</FileDropzone>
		<Table source={tableSimple} />
	</div>
</div>
