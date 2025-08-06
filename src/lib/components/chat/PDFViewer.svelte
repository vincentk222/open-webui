<script lang="ts">
  import { WEBUI_API_BASE_URL } from '$lib/constants';
  export let file: any = null;
  $: isPDF =
    file &&
    (file?.meta?.content_type === 'application/pdf' ||
      file?.name?.toLowerCase().endsWith('.pdf'));
  $: src = isPDF ? `${WEBUI_API_BASE_URL}/files/${file.id}/content` : null;
</script>

{#if src}
  <iframe title={file?.name} src={src} class="w-full h-full border-0" />
{:else}
  <div class="w-full h-full flex items-center justify-center text-gray-500">
    No PDF selected
  </div>
{/if}
