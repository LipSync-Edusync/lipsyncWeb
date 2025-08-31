<script lang="ts">
	export let title: string;
	export let onProcess: (file: File | null, sourceLang: string, destLang: string) => void;

	let file: File | null = null;
	let videoUrl: string | null = null;
	let sourceLang = 'en';
	let destLang = 'es';

	function handleFileChange(e: Event) {
		const target = e.target as HTMLInputElement;
		if (target.files && target.files.length > 0) {
			file = target.files[0];
			videoUrl = URL.createObjectURL(file);
		}
	}

	function handleProcess() {
		onProcess(file, sourceLang, destLang);
	}
</script>

<div
	class="bg-slate-900 text-gray-200 rounded-2xl shadow-xl p-6 space-y-6 border border-slate-700 
	hover:border-blue-500 transition-all duration-300 hover:shadow-blue-500/30 hover:scale-[1.02]"
>
	<h2 class="text-xl font-bold text-blue-400">{title}</h2>

	<!-- File Upload -->
	<div>
		<label class="block font-medium mb-2">🎬 Upload Your File</label>
		<input
			type="file"
			accept=".mp4,.wav"
			on:change={handleFileChange}
			class="block w-full rounded-lg border border-slate-700 bg-slate-800 text-gray-300 p-2
			       focus:outline-none focus:ring-2 focus:ring-blue-500 transition"
		/>
	</div>

	<!-- Video Preview -->
	{#if videoUrl}
		<div class="rounded-lg overflow-hidden shadow-lg border border-slate-700">
			<video src={videoUrl} controls class="w-full" />
		</div>
	{/if}

	<!-- Language Selection -->
	<div class="grid grid-cols-2 gap-4">
		<div>
			<label class="block font-medium mb-2">🌐 Source Language</label>
			<select
				bind:value={sourceLang}
				class="w-full rounded-lg border border-slate-700 bg-slate-800 text-gray-300 p-2
				       focus:outline-none focus:ring-2 focus:ring-blue-500 transition"
			>
				<option value="en">English</option>
				<option value="hi">Hindi</option>
				<option value="fr">French</option>
			</select>
		</div>
		<div>
			<label class="block font-medium mb-2">🎯 Destination Language</label>
			<select
				bind:value={destLang}
				class="w-full rounded-lg border border-slate-700 bg-slate-800 text-gray-300 p-2
				       focus:outline-none focus:ring-2 focus:ring-pink-500 transition"
			>
				<option value="es">Spanish</option>
				<option value="de">German</option>
				<option value="ja">Japanese</option>
			</select>
		</div>
	</div>

	<!-- Action Button -->
	<button
		class="bg-gradient-to-r from-blue-600 to-pink-600 text-white font-semibold px-4 py-2 
		       rounded-lg shadow-lg hover:from-blue-500 hover:to-pink-500 w-full
		       transition transform hover:scale-[1.03] active:scale-95"
		on:click|preventDefault={handleProcess}
		disabled={!file}
	>
		⚡ Process Video
	</button>
</div>
