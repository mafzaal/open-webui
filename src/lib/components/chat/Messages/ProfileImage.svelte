<script lang="ts">
	import { settings } from '$lib/stores';
	import ProfileImageBase from './ProfileImageBase.svelte';

	interface Props {
		className?: string;
		src?: string;
	}

	let { className = 'size-8', src = '' }: Props = $props();
</script>

<div class={`flex-shrink-0 ${($settings?.chatDirection ?? 'LTR') === 'LTR' ? 'mr-3' : 'ml-3'}`}>
	<ProfileImageBase {src} {className} />
</div>
