<script lang="ts">
  import Card from '$lib/components/ui/card/Card.svelte';

  let videoFile: File | null = null;
  let sourceLang = 'en';
  let destLang = 'hi';
  let loading = false;
  let resultVideoUrl = '';
  let error = '';

  const languages = [
    { label: 'English', value: 'en' },
    { label: 'Spanish', value: 'es' },
    { label: 'Hindi', value: 'hi' },
    { label: 'French', value: 'fr' }
  ];

  async function handleUpload() {
    error = '';
    if (!videoFile) {
      error = 'Please select a video file.';
      return;
    }
    loading = true;
    const formData = new FormData();
    formData.append('video', videoFile);
    formData.append('sr_lang', sourceLang);
    formData.append('dest_lang', destLang);
    formData.append('utc_str', Date.now().toString());

    try {
      const res = await fetch('http://127.0.0.1:5000/upload', {
        method: 'POST',
        body: formData
      });
      const result = await res.json();
      if (!res.ok) throw new Error(result.error || 'Upload failed');
      resultVideoUrl = `http://127.0.0.1:5000${result.video_url}`;
    } catch (e) {
      error = e.message;
    } finally {
      loading = false;
    }
  }
</script>

<div class="min-h-screen mt-10 bg-gray-50 text-gray-800 p-6 flex flex-col lg:flex-row gap-6">
  <!-- Left Card -->
  <Card title="Upload Video" subtitle="Step 1">
    {#if error}
      <div class="mb-2 text-red-600 text-sm">{error}</div>
    {/if}
    <input type="file"
      class="w-full text-sm border border-gray-300 rounded-md p-2 bg-white text-gray-700"
      accept="video/mp4"
      on:change={e => videoFile = e.target.files[0]} />

    <div>
      <label class="block text-sm mb-1 text-gray-600">Source Language</label>
      <select
        class="w-full p-2 rounded-md border border-gray-300 bg-white text-gray-700"
        bind:value={sourceLang}>
        {#each languages as lang}
          <option value={lang.value}>{lang.label}</option>
        {/each}
      </select>
    </div>

    <div>
      <label class="block text-sm mb-1 text-gray-600">Destination Language</label>
      <select
        class="w-full p-2 rounded-md border border-gray-300 bg-white text-gray-700"
        bind:value={destLang}>
        {#each languages as lang}
          <option value={lang.value}>{lang.label}</option>
        {/each}
      </select>
    </div>

    <button
      class="w-full py-2 mt-2 rounded-md bg-gray-800 text-white hover:bg-gray-700 transition"
      on:click={handleUpload}
      disabled={loading}>
      {loading ? 'Processing...' : 'Process Video'}
    </button>
  </Card>

  <!-- Right Card -->
  <Card title="Processing / Result" subtitle="Step 2">
    {#if loading}
      <div class="flex justify-center items-center">
        <div class="relative w-12 h-12">
          <div class="absolute inset-0 rounded-full border-4 border-gray-200"></div>
          <div class="absolute inset-0 rounded-full border-4 border-t-gray-600 animate-spin"></div>
        </div>
      </div>
      <p class="text-center text-gray-500">Processing your video...</p>
    {:else if resultVideoUrl}
      <video controls class="w-full rounded-md mt-3 border border-gray-300">
        <source src={resultVideoUrl} type="video/mp4" />
      </video>
      <a href={resultVideoUrl} download
        class="block text-center w-full mt-3 py-2 rounded-md bg-white text-gray-800 border border-gray-300 hover:bg-gray-100 transition">
        Download Video
      </a>
    {:else}
      <p class="text-center text-gray-500">No video processed yet.</p>
    {/if}
  </Card>
</div>
