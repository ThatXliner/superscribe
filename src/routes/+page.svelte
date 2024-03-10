<script lang="ts">
	import MermaidGraph from '$lib/MermaidGraph.svelte';
</script>

<h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>

<MermaidGraph
	graph="
graph LR
A --- B
B-->C[fa:fa-ban forbidden]
B-->D(arst fa:fa-spinner);"
/>
