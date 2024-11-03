<script lang="ts">
	import { run } from 'svelte/legacy';

	import { onMount, tick } from 'svelte';



	interface Props {
		value?: string;
		placeholder?: string;
		rows?: number;
		required?: boolean;
		className?: string;
	}

	let {
		value = $bindable(''),
		placeholder = '',
		rows = 1,
		required = false,
		className = 'w-full rounded-lg px-3 py-2 text-sm bg-gray-50 dark:text-gray-300 dark:bg-gray-850 outline-none resize-none h-full'
	}: Props = $props();

	let textareaElement = $state();

	onMount(async () => {
		await tick();
		if (textareaElement) {
			await tick();
			setTimeout(adjustHeight, 0);
		}
	});


	const adjustHeight = () => {
		if (textareaElement) {
			textareaElement.style.height = '';
			textareaElement.style.height = `${textareaElement.scrollHeight}px`;
		}
	};
	run(() => {
		if (value) {
			setTimeout(adjustHeight, 0);
		}
	});
</script>

<textarea
	bind:this={textareaElement}
	bind:value
	{placeholder}
	oninput={adjustHeight}
	onfocus={adjustHeight}
	class={className}
	{rows}
	{required}
></textarea>
