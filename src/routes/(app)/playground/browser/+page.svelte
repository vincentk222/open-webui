<script>
    import { getContext } from 'svelte';
    const i18n = getContext('i18n');
</script>

<div class="p-4 text-center">
    <h1 class="text-2xl font-semibold">{$i18n.t('Browser')}</h1>
    <p class="text-gray-500">This is the Browser page.</p>
</div>
