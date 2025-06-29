<script lang="ts">
	import { shortcut, shortcuts } from '$lib/components/ShortcutHelpModal.svelte';
	import { ChevronLeft, ChevronRight } from 'lucide-svelte';

	interface Props {
		itemID: number;
		itemsQueue: number[];
		action: 'next' | 'previous';
	}
	let { itemID, itemsQueue, action }: Props = $props();

	let currentIndex = $derived(itemsQueue.findIndex((id) => id === itemID));
	let prevID = $derived(itemsQueue[currentIndex - 1] ?? itemsQueue[0]);
	let nextID = $derived(itemsQueue[currentIndex + 1] ?? itemsQueue.at(-1));
	let goto = $derived((action === 'previous' ? prevID : nextID) ?? itemID);

	// check if there are other items to navigate to
	let hasOtherItems = $derived(itemsQueue.length > 1);
	let hasValidTarget = $derived(goto !== itemID);

	let visible = $state(true);
	let lastScrollY = $state(0);

	$effect(() => {
		function handleScroll() {
			const currentScrollY = window.scrollY;

			// Always hide if there are no other items or no valid target
			if (!hasOtherItems || !hasValidTarget) {
				visible = false;
				return;
			}

			// show when scrolling up or at the top
			if (currentScrollY < lastScrollY || currentScrollY < 10) {
				visible = true;
			} else if (currentScrollY > lastScrollY && currentScrollY > 100) {
				// hide when scrolling down (after 100px)
				visible = false;
			}

			lastScrollY = currentScrollY;
		}

		window.addEventListener('scroll', handleScroll, { passive: true });
		return () => window.removeEventListener('scroll', handleScroll);
	});
</script>

<a
	href={'/items/' + goto}
	use:shortcut={action === 'previous' ? shortcuts.prevItem.keys : shortcuts.nextItem.keys}
	class="btn lg:btn-ghost btn-circle lg:btn-xl fixed bottom-6
	    {action === 'previous' ? 'left-4' : 'right-4'} lg:sticky lg:top-[50%]
		{hasOtherItems && hasValidTarget && visible ? 'visible' : 'invisible'} lg:visible"
>
	{#if action === 'previous'}
		<ChevronLeft />
	{:else}
		<ChevronRight />
	{/if}
</a>
