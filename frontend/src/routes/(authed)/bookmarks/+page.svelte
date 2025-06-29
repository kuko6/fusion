<script lang="ts">
	import ItemList from '$lib/components/ItemList.svelte';
	import PageNavHeader from '$lib/components/PageNavHeader.svelte';
	import ItemActionMarkAllasRead from '$lib/components/ItemActionMarkAllasRead.svelte';
	import { t } from '$lib/i18n';

	let { data } = $props();
</script>

<svelte:head>
	<title>{t('common.bookmark')}</title>
</svelte:head>

<div class="flex flex-col">
    <PageNavHeader showSearch={true}>
		{#await data.items}
			<ItemActionMarkAllasRead disabled />
		{:then items}
			<ItemActionMarkAllasRead items={items.items} />
		{/await}
	</PageNavHeader>
	<div class="px-4 lg:px-8">
		<div class="py-6">
			<h1 class="text-3xl font-bold">{t('common.bookmark')}</h1>
		</div>
		<ItemList data={data.items} />
	</div>
</div>
