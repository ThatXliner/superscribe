<script lang="ts">
	import mermaid from 'mermaid';
	import { onMount } from 'svelte';
	mermaid.initialize({ startOnLoad: true });
	const { graph }: { graph: string } = $props();
	let ref: HTMLPreElement;
	onMount(async () => {
		await mermaid.run({ nodes: [ref] });
	});
</script>

<pre bind:this={ref}>
    {graph}
</pre>
