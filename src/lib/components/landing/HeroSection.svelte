<script lang="ts">
	import { ArrowRightIcon } from 'lucide-svelte';
	import AnimatedShinyText from '$lib/components/magic/AnimatedShinyText/AnimatedShinyText.svelte';
	import BorderBeam from '$lib/components/magic/borderbeam/BorderBeam.svelte';
	import Button from '$lib/components/ui/button/button.svelte';
	import type { ObserverEventDetails } from 'svelte-inview';
	import { inview } from 'svelte-inview';

	// Images
	import HeroDarkImg from '$lib/imgs/hero-dark.png';
	import HeroLightImg from '$lib/imgs/hero-light.png';
	let inView = false;
	const handleChange = ({ detail }: CustomEvent<ObserverEventDetails>) => {
		// console.log(detail);
		inView = detail.inView;
	};
</script>

<section id="hero" class="relative mx-auto mt-32 max-w-7xl px-6 text-center md:px-8">
	<div
		class="group inline-flex h-7 -translate-y-4 animate-fade-in items-center justify-between gap-1 rounded-full border border-gray-200 bg-white px-3 text-xs text-gray-900 opacity-0 transition-all ease-in hover:cursor-pointer hover:bg-gray-100"
	>
		<AnimatedShinyText class="inline-flex items-center justify-center">
			<span>✨ Browse the Demos</span>
			{' '}
			<ArrowRightIcon
				class="ml-1 size-3 transition-transform duration-300 ease-in-out group-hover:translate-x-0.5"
			/>
		</AnimatedShinyText>
	</div>
	<h1
		class="-translate-y-4 animate-fade-in text-balance bg-gradient-to-br from-gray-900 from-30% to-gray-500 bg-clip-text py-6 text-5xl font-medium leading-none tracking-tighter text-transparent opacity-0 [--animation-delay:200ms] sm:text-6xl md:text-7xl lg:text-8xl"
	>
		Multilingua Video Translation
		<br class="hidden md:block" />
		{' '}
		with Lipsynchronization
	</h1>
	<p
		class="mb-12 -translate-y-4 animate-fade-in text-balance text-lg tracking-tight text-gray-600 opacity-0 [--animation-delay:400ms] md:text-xl"
	>
		Deep Learning based multipurpose Framework built with
		<br class="hidden md:block" />
		{' '}
		Python, <a href="https://pytorch.org/" class="underline underline-offset-2 text-gray-900">PyTorch</a>, and
		<a href="https://svelte.dev/" class="underline underline-offset-2 text-gray-900">Svelte</a>.
	</p>
	<Button
		on:click={() => window.location.href = '/dashboard'}
		class="-translate-y-4 animate-fade-in gap-1 rounded-lg bg-white text-gray-900 border border-gray-200 opacity-0 ease-in-out [--animation-delay:600ms] hover:bg-gray-100"
	>
		<span>Dashboard </span>
		<ArrowRightIcon
			class="ml-1 size-4 transition-transform duration-300 ease-in-out group-hover:translate-x-1"
		/>
	</Button>
	<div
		use:inview={{
			unobserveOnEnter: true,
			rootMargin: '-100px'
		}}
		on:inview_change={handleChange}
		class="relative mt-32 animate-fade-up opacity-0 [--animation-delay:400ms] [perspective:2000px] after:absolute after:inset-0 after:z-50 after:[background:linear-gradient(to_top,#fff_30%,transparent)]"
	>
		<div
			class="rounded-xl border border-gray-200 bg-white bg-opacity-100 before:absolute before:bottom-1/2 before:left-0 before:top-0 before:size-full before:opacity-0 before:[background-image:linear-gradient(to_bottom,#f3f4f6,#e5e7eb,transparent_40%)] before:[filter:blur(180px)] {inView
				? 'before:animate-image-glow'
				: ''}"
		>
			<BorderBeam
				size={200}
				duration={12}
				delay={0}
				colorFrom="#f3f4f6"
				colorTo="#e5e7eb"
			/>

			<img
				src={HeroLightImg}
				alt="HeroLightImage"
				class="relative block size-full rounded-[inherit] border border-gray-200 object-contain"
			/>
		</div>
	</div>
</section>
