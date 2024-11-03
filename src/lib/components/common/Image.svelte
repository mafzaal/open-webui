<script lang="ts">
	import { run } from 'svelte/legacy';

	import { WEBUI_BASE_URL } from '$lib/constants';
	import ImagePreview from './ImagePreview.svelte';


	interface Props {
		src?: string;
		alt?: string;
		className?: string;
	}

	let { src = '', alt = '', className = ' w-full' }: Props = $props();

	let _src = $state('');
	run(() => {
		_src = src.startsWith('/') ? `${WEBUI_BASE_URL}${src}` : src;
	});

	let showImagePreview = $state(false);
</script>

<button
	class={className}
	onclick={() => {
		showImagePreview = true;
	}}
>
	<img src={_src} {alt} class=" rounded-lg cursor-pointer" draggable="false" data-cy="image" />
</button>

<ImagePreview bind:show={showImagePreview} src={_src} {alt} />
