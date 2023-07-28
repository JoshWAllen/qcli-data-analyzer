<script lang="ts">
	import { FileDropzone } from '@skeletonlabs/skeleton';
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
</script>

<div class="container h-full mx-auto flex justify-center items-center">
	<div class="w-2/3 space-y-5">
		<h1 class="h1">Dashboard</h1>
		<FileDropzone name="files" accept=".csv" bind:files on:change={onChangeHandler}>
			<svelte:fragment slot="lead">(icon)</svelte:fragment>
			<svelte:fragment slot="message">Upload a file or drag and drop</svelte:fragment>
			<svelte:fragment slot="meta">only CSV allowed</svelte:fragment>
		</FileDropzone>
	</div>
</div>
