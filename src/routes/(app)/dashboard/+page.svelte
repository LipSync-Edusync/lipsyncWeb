<script>
  import Card from '$lib/components/ui/card/Card.svelte';
</script>

<div class="min-h-screen mt-10 bg-gray-50 text-gray-800 p-6 flex flex-col lg:flex-row gap-6">
  
  <!-- Left Card -->
  <Card title="Upload Video" subtitle="Step 1">
    <input type="file"
      class="w-full text-sm border border-gray-300 rounded-md p-2 bg-white text-gray-700" />

    <div>
      <label class="block text-sm mb-1 text-gray-600">Source Language</label>
      <select
        class="w-full p-2 rounded-md border border-gray-300 bg-white text-gray-700">
        <option>English</option>
        <option>Spanish</option>
      </select>
    </div>

    <div>
      <label class="block text-sm mb-1 text-gray-600">Destination Language</label>
      <select
        class="w-full p-2 rounded-md border border-gray-300 bg-white text-gray-700">
        <option>Hindi</option>
        <option>French</option>
      </select>
    </div>

    <button
      class="w-full py-2 mt-2 rounded-md bg-gray-800 text-white hover:bg-gray-700 transition">
      Process Video
    </button>
  </Card>

  <!-- Right Card -->
  <Card title="Processing" subtitle="Step 2">
    <div class="flex justify-center items-center">
      <div class="relative w-12 h-12">
        <div class="absolute inset-0 rounded-full border-4 border-gray-200"></div>
        <div class="absolute inset-0 rounded-full border-4 border-t-gray-600 animate-spin"></div>
      </div>
    </div>

    <p class="text-center text-gray-500">Processing your video...</p>

    <video controls class="w-full rounded-md mt-3 border border-gray-300">
      <source src="/sample.mp4" type="video/mp4" />
    </video>

    <a href="/sample.mp4" download
      class="block text-center w-full mt-3 py-2 rounded-md bg-white text-gray-800 border border-gray-300 hover:bg-gray-100 transition">
      Download Video
    </a>
  </Card>
</div>
