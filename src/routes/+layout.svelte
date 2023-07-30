<script lang="ts">
	// The ordering of these imports is critical to your app working properly
	import '@skeletonlabs/skeleton/themes/theme-skeleton.css';
	// If you have source.organizeImports set to true in VSCode, then it will auto change this ordering
	import '@skeletonlabs/skeleton/styles/skeleton.css';
	// Most of your app wide CSS should be put in this file
	import '../app.postcss';
	import {
		AppBar,
		AppRail,
		AppRailAnchor,
		AppRailTile,
		AppShell,
		LightSwitch
	} from '@skeletonlabs/skeleton';
	import { links } from '$lib/links';

	let currentTile = 0;
</script>

<AppShell>
	<svelte:fragment slot="header">
		<AppBar class="w-full">
			<svelte:fragment slot="lead">
				<h1>QCLI</h1>
			</svelte:fragment>
			<nav>
				<ul class="flex space-x-2">
					{#each links.navlinks as link}
						<li><a href={link.url} class="anchor">{link.title}</a></li>
					{/each}
				</ul>
			</nav>
			<svelte:fragment slot="trail">
				<LightSwitch />
			</svelte:fragment>
		</AppBar>
	</svelte:fragment>
	<svelte:fragment slot="sidebarLeft">
		<AppRail background="bg-surface-50-900-token">
			<hr class="opacity-30" />
			<AppRailTile bind:group={currentTile} name="tile-1" value={0} title="tile-1">
				<svelte:fragment slot="lead">(icon)</svelte:fragment>
				<span>File Explorer</span>
			</AppRailTile>
			<AppRailTile bind:group={currentTile} name="tile-2" value={1} title="tile-2">
				<svelte:fragment slot="lead">(icon)</svelte:fragment>
				<span>Gallery</span>
			</AppRailTile>
			<AppRailTile bind:group={currentTile} name="tile-3" value={2} title="tile-3">
				<svelte:fragment slot="lead">(icon)</svelte:fragment>
				<span>Settings</span>
			</AppRailTile>
		</AppRail>
	</svelte:fragment>
	<slot />
</AppShell>
