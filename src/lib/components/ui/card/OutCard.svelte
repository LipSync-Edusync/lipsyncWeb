<script lang="ts">
	export let processing = false;
	export let videoUrl: string | null = null;
</script>

<div
	class="bg-slate-900 text-gray-200 rounded-2xl shadow-xl p-6 flex flex-col items-center justify-center 
	       border border-slate-700 hover:border-green-500 transition-all duration-300 hover:shadow-green-500/30 hover:scale-[1.02]"
>
	<h2 class="text-xl font-bold text-green-400 w-full border-b border-slate-700 pb-2 mb-4">Output</h2>

	{#if processing}
		<!-- Neon Spinner -->
		<div class="relative w-32 h-32 mt-6">
			<div class="absolute inset-0 rounded-full border-4 border-slate-700"></div>
			<div
				class="absolute inset-0 rounded-full border-4 border-green-500 border-t-transparent 
				       animate-spin"
			></div>
			<div class="absolute inset-0 flex items-center justify-center text-green-400 font-semibold">
				Processing...
			</div>
		</div>
	{:else if videoUrl}
		<div class="space-y-4 text-center w-full mt-6">
			<div class="rounded-lg overflow-hidden shadow-lg border border-slate-700">
				<video src={videoUrl} controls class="w-full" />
			</div>
			<a
				href={videoUrl}
				download="processed-video.mp4"
				class="bg-gradient-to-r from-green-600 to-blue-600 text-white font-semibold px-4 py-2 rounded-lg 
				       shadow-lg hover:from-green-500 hover:to-blue-500 inline-block transition transform hover:scale-[1.03] active:scale-95"
			>
				⬇ Download Video
			</a>
		</div>
	{:else}
		<p class="text-gray-500 mt-6">Your processed video will appear here</p>
	{/if}
</div>

<style>
	@keyframes spin {
		to {
			transform: rotate(360deg);
		}
	}
	.animate-spin {
		animation: spin 1s linear infinite;
	}
</style>
