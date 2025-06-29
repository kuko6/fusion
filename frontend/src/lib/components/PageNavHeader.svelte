<script lang="ts">
	import { Menu } from 'lucide-svelte';
	import type { Snippet } from 'svelte';
	import ActionSearch from './ActionSearch.svelte';

	interface Props {
		children?: Snippet;
		showSearch?: boolean;
		title?: string;
	}

	let { title, children, showSearch }: Props = $props();
</script>

<header class="bg-base-100 border-neutral sticky top-0 z-50 py-2 {children ? 'border-b' : ''}">
	<div class="flex flex-col justify-between px-1 md:px-4 lg:flex-row lg:items-center lg:px-8">
		<div class="flex items-center justify-between gap-4">
			<label for="sidebar-toggle" class="btn btn-ghost btn-square lg:hidden">
				<Menu class="size-4" />
			</label>
			{#if showSearch}
				<ActionSearch />
			{/if}
		</div>
		<div class="ml-auto">
			{@render children?.()}
		</div>
	</div>
</header>
