<script lang="ts">
	import { run } from 'svelte/legacy';

	import { onMount, getContext } from 'svelte';
	const i18n = getContext('i18n');

	import panzoom from 'panzoom';

	import DOMPurify from 'dompurify';
	import DocumentDuplicate from '../icons/DocumentDuplicate.svelte';
	import { copyToClipboard } from '$lib/utils';
	import { toast } from 'svelte-sonner';
	import Tooltip from './Tooltip.svelte';
	import Clipboard from '../icons/Clipboard.svelte';

	interface Props {
		className?: string;
		svg?: string;
		content?: string;
	}

	let { className = '', svg = '', content = '' }: Props = $props();

	let instance = $state();

	let sceneParentElement: HTMLElement = $state();
	let sceneElement: HTMLElement = $state();

	run(() => {
		if (sceneElement) {
			instance = panzoom(sceneElement, {
				bounds: true,
				boundsPadding: 0.1,

				zoomSpeed: 0.065
			});
		}
	});
</script>

<div bind:this={sceneParentElement} class="relative {className}">
	<div bind:this={sceneElement} class="flex h-full max-h-full justify-center items-center">
		{@html svg}
	</div>

	{#if content}
		<div class=" absolute top-1 right-1">
			<Tooltip content={$i18n.t('Copy to clipboard')}>
				<button
					class="p-1.5 rounded-lg border border-gray-100 dark:border-none dark:bg-gray-850 hover:bg-gray-50 dark:hover:bg-gray-800 transition"
					onclick={() => {
						copyToClipboard(content);
						toast.success($i18n.t('Copied to clipboard'));
					}}
				>
					<Clipboard className=" size-4" strokeWidth="1.5" />
				</button>
			</Tooltip>
		</div>
	{/if}
</div>
