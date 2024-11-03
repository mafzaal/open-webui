<script lang="ts">
	import { toast } from 'svelte-sonner';

	import { tick, getContext, onMount, createEventDispatcher } from 'svelte';
	const dispatch = createEventDispatcher();
	const i18n = getContext('i18n');

	import { settings } from '$lib/stores';
	import { copyToClipboard } from '$lib/utils';

	import MultiResponseMessages from './MultiResponseMessages.svelte';
	import ResponseMessage from './ResponseMessage.svelte';
	import UserMessage from './UserMessage.svelte';







	

	interface Props {
		chatId: any;
		idx?: number;
		history: any;
		messageId: any;
		user: any;
		showPreviousMessage: any;
		showNextMessage: any;
		editMessage: any;
		deleteMessage: any;
		rateMessage: any;
		regenerateResponse: any;
		continueResponse: any;
		// MultiResponseMessages
		mergeResponses: any;
		autoScroll?: boolean;
		readOnly?: boolean;
	}

	let {
		chatId,
		idx = 0,
		history = $bindable(),
		messageId,
		user,
		showPreviousMessage,
		showNextMessage,
		editMessage,
		deleteMessage,
		rateMessage,
		regenerateResponse,
		continueResponse,
		mergeResponses,
		autoScroll = false,
		readOnly = false
	}: Props = $props();

	onMount(() => {
		// console.log('message', idx);
	});
</script>

<div
	class="flex flex-col justify-between px-5 mb-3 w-full {($settings?.widescreenMode ?? null)
		? 'max-w-full'
		: 'max-w-5xl'} mx-auto rounded-lg group"
>
	{#if history.messages[messageId]}
		{#if history.messages[messageId].role === 'user'}
			<UserMessage
				{user}
				{history}
				{messageId}
				isFirstMessage={idx === 0}
				siblings={history.messages[messageId].parentId !== null
					? (history.messages[history.messages[messageId].parentId]?.childrenIds ?? [])
					: (Object.values(history.messages)
							.filter((message) => message.parentId === null)
							.map((message) => message.id) ?? [])}
				{showPreviousMessage}
				{showNextMessage}
				{editMessage}
				on:delete={() => deleteMessage(messageId)}
				{readOnly}
			/>
		{:else if (history.messages[history.messages[messageId].parentId]?.models?.length ?? 1) === 1}
			<ResponseMessage
				{chatId}
				{history}
				{messageId}
				isLastMessage={messageId === history.currentId}
				siblings={history.messages[history.messages[messageId].parentId]?.childrenIds ?? []}
				{showPreviousMessage}
				{showNextMessage}
				{editMessage}
				{rateMessage}
				{continueResponse}
				{regenerateResponse}
				on:submit={async (e) => {
					dispatch('submit', e.detail);
				}}
				on:action={async (e) => {
					dispatch('action', e.detail);
				}}
				on:update={async (e) => {
					dispatch('update');
				}}
				on:save={async (e) => {
					console.log('save', e);

					const message = e.detail;
					if (message) {
						history.messages[message.id] = message;
						dispatch('update');
					} else {
						dispatch('update');
					}
				}}
				{readOnly}
			/>
		{:else}
			<MultiResponseMessages
				bind:history
				{chatId}
				{messageId}
				isLastMessage={messageId === history?.currentId}
				{rateMessage}
				{editMessage}
				{continueResponse}
				{regenerateResponse}
				{mergeResponses}
				on:submit={async (e) => {
					dispatch('submit', e.detail);
				}}
				on:action={async (e) => {
					dispatch('action', e.detail);
				}}
				on:update={async (e) => {
					dispatch('update');
				}}
				on:save={async (e) => {
					console.log('save', e);
					const message = e.detail;
					if (message) {
						history.messages[message.id] = message;
						dispatch('update');
					} else {
						dispatch('update');
					}
				}}
				on:change={async () => {
					await tick();
					dispatch('update');
					dispatch('scroll');
				}}
				{readOnly}
			/>
		{/if}
	{/if}
</div>
