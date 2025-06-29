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

	let headerVisible = $state(true);
	let lastScrollY = $state(0);

	$effect(() => {
		function handleScroll() {
			const currentScrollY = window.scrollY;

			// show header when scrolling up or at the top
			if (currentScrollY < lastScrollY || currentScrollY < 10) {
				headerVisible = true;
			} else if (currentScrollY > lastScrollY && currentScrollY > 100) {
				// hide header when scrolling down (after 100px)
				headerVisible = false;
			}

			lastScrollY = currentScrollY;
		}

		window.addEventListener('scroll', handleScroll, { passive: true });
		return () => window.removeEventListener('scroll', handleScroll);
	});
</script>

<header class="bg-base-100 border-neutral sticky top-0 z-50 py-2 transition-transform duration-300 {children ? 'border-b' : ''} {headerVisible ? 'translate-y-0' : '-translate-y-full'} lg:translate-y-0">
	<div class="flex flex-col justify-between px-1 md:px-4 flex-row items-center">
   	    <div class="flex items-center justify-start gap-4 flex-1">
			<label for="sidebar-toggle" class="btn btn-ghost btn-square lg:hidden">
				<Menu class="size-4" />
			</label>
			{#if showSearch}
				<ActionSearch />
			{/if}
			{#if title}
				<span class="text-base-content/60 text-sm">{title}</span>
			{/if}
		</div>
		<div class="flex flex-row flex-shrink-0">
			{@render children?.()}
		</div>
	</div>
</header>
