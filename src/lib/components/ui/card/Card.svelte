<script lang="ts">
  export let title: string;
  export let subtitle: string;
</script>

<div
  class="w-full max-w-md rounded-xl p-6 bg-white border border-gray-200 shadow-sm">
  
  <!-- Header -->
  <div class="flex justify-between items-center mb-4">
    <h2 class="text-lg font-semibold text-gray-800">{title}</h2>
    {#if subtitle}
      <span class="text-xs px-2 py-1 rounded-md bg-gray-100 text-gray-600 border border-gray-300">
        {subtitle}
      </span>
    {/if}
  </div>

  <!-- Slot for content -->
  <div class="space-y-3">
    <slot />
  </div>
</div>
