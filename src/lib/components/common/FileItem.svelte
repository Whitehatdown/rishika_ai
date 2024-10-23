<script lang="ts">
	import { createEventDispatcher, getContext } from 'svelte';
	import { formatFileSize } from '$lib/utils';

	import FileItemModal from './FileItemModal.svelte';
	import GarbageBin from '../icons/GarbageBin.svelte';
	import Spinner from './Spinner.svelte';

	const i18n = getContext('i18n');
	const dispatch = createEventDispatcher();

	export let className = 'w-60';
	export let colorClassName = 'bg-white dark:bg-gray-850 border border-gray-50 dark:border-white/5';
	export let url: string | null = null;

	export let dismissible = false;
	export let loading = false;

	export let item = null;
	export let edit = false;

	export let name: string;
	export let type: string;
	export let size: number;

	let showModal = false;
</script>

{#if item}
	<FileItemModal bind:show={showModal} bind:item {edit} />
{/if}

<button
	class="relative group p-1.5 {className} flex items-center {colorClassName} rounded-2xl text-left"
	type="button"
	on:click={async () => {
		if (item?.file?.data?.content) {
			showModal = !showModal;
		} else {
			if (url) {
				if (type === 'file') {
					window.open(`${url}/content`, '_blank').focus();
				} else {
					window.open(`${url}`, '_blank').focus();
				}
			}
		}}
	>
		<div class="p-4 py-[1.1rem] bg-[#000847] text-white rounded-l-xl">
			{#if status === 'processed'}
				<svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 24 24"
					fill="currentColor"
					class=" size-5"
				>
					<path
						fill-rule="evenodd"
						d="M12 2C6.477 2 2 6.477 2 12s4.477 10 10 10 10-4.477 10-10S17.523 2 12 2zm0 18a8 8 0 1 1 0-16 8 8 0 0 1 0 16zm3-4c-.223-1.31-1.622-2.5-3-2.5s-2.777 1.19-3 2.5H7a8.026 8.026 0 0 1 10 0h-2zm-6-3a8.025 8.025 0 0 1 2-1.464V11c-1.378-.746-2.777-2-3-2H7a8.026 8.026 0 0 1 0 4h2zm6-4c.223 1.31 1.622 2.5 3 2.5s2.777-1.19 3-2.5H17a8.025 8.025 0 0 1-2-1.464V11c1.378.746 2.777 2 3 2h2a8.026 8.026 0 0 1 0-4h-2z"
						clip-rule="evenodd"
					/>
					<path
						d="M12.971 1.816A5.23 5.23 0 0 1 14.25 5.25v1.875c0 .207.168.375.375.375H16.5a5.23 5.23 0 0 1 3.434 1.279 9.768 9.768 0 0 0-6.963-6.963Z"
					/>
				</svg>
			{:else}
				<svg
					class=" size-5 translate-y-[0.5px]"
					fill="currentColor"
					viewBox="0 0 24 24"
					xmlns="http://www.w3.org/2000/svg"
				>
					<style>
						.spinner_qM83 {
							animation: spinner_8HQG 1.05s infinite;
						}
						.spinner_oXPr {
							animation-delay: 0.1s;
						}
						.spinner_ZTLf {
							animation-delay: 0.2s;
						}
						@keyframes spinner_8HQG {
							0%, 57.14% {
								animation-timing-function: cubic-bezier(0.33, 0.66, 0.66, 1);
								transform: translate(0);
							}
							28.57% {
								animation-timing-function: cubic-bezier(0.33, 0, 0.66, 0.33);
								transform: translateY(-6px);
							}
							100% {
								transform: translate(0);
							}
						}
					</style>
					<circle class="spinner_qM83" cx="4" cy="12" r="2.5" />
					<circle class="spinner_qM83 spinner_oXPr" cx="12" cy="12" r="2.5" />
					<circle class="spinner_qM83 spinner_ZTLf" cx="20" cy="12" r="2.5" />
				</svg>
			{/if}
		</div>


		<div class="flex flex-col justify-center -space-y-0.5 pl-1.5 pr-4 w-full">
			<div class=" dark:text-gray-100 text-sm font-medium line-clamp-1 mb-1">
				{name}
			</div>

			<div class=" flex justify-between text-gray-500 text-xs">
				{#if type === 'file'}
					{$i18n.t('File')}
				{:else if type === 'doc'}
					{$i18n.t('Document')}
				{:else if type === 'collection'}
					{$i18n.t('Collection')}
				{:else}
					<span class=" capitalize">{type}</span>
				{/if}
				{#if size}
					<span class="capitalize">{formatSize(size)}</span>
				{/if}
			</div>
		</div>
	</button>

	{#if dismissible}
		<div class=" absolute -top-1 -right-1">
			<button
				class=" bg-gray-400 text-white border border-white rounded-full group-hover:visible invisible transition"
				type="button"
				on:click|stopPropagation={() => {
					dispatch('dismiss');
				}}
			>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					viewBox="0 0 20 20"
					fill="currentColor"
					class="w-4 h-4"
				>
					<path
						d="M6.28 5.22a.75.75 0 00-1.06 1.06L8.94 10l-3.72 3.72a.75.75 0 101.06 1.06L10 11.06l3.72 3.72a.75.75 0 101.06-1.06L11.06 10l3.72-3.72a.75.75 0 00-1.06-1.06L10 8.94 6.28 5.22z"
					/>
				</svg>
			</button>

			<!-- <button
				class=" p-1 dark:text-gray-300 dark:hover:text-white hover:bg-black/5 dark:hover:bg-white/5 rounded-full group-hover:visible invisible transition"
				type="button"
				on:click={() => {
				}}
			>
				<GarbageBin />
			</button> -->
		</div>
	{/if}
</button>
