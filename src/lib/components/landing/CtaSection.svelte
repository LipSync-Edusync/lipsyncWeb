<script lang="ts">
	import { BarChart, ChevronRight, File, Globe, HeartHandshake, Rss, Shield } from 'lucide-svelte';
	import Marquee from '../magic/marquee/Marquee.svelte';
	import CtaCard from './CtaCard.svelte';
	import Button from '../ui/button/button.svelte';
	let tiles: { icon: any; bg: string }[] = [
		{
			icon: HeartHandshake,
			bg: 'pointer-events-none absolute left-1/2 top-1/2 size-1/2 -translate-x-1/2 -translate-y-1/2 overflow-visible rounded-full bg-white opacity-60 blur-[20px]'
		},
		{
			icon: Globe,
			bg: 'pointer-events-none absolute left-1/2 top-1/2 size-1/2 -translate-x-1/2 -translate-y-1/2 overflow-visible rounded-full bg-gray-100 opacity-60 blur-[20px]'
		},
		{
			icon: File,
			bg: 'pointer-events-none absolute left-1/2 top-1/2 size-1/2 -translate-x-1/2 -translate-y-1/2 overflow-visible rounded-full bg-gray-200 opacity-60 blur-[20px]'
		},
		{
			icon: Shield,
			bg: 'pointer-events-none absolute left-1/2 top-1/2 size-1/2 -translate-x-1/2 -translate-y-1/2 overflow-visible rounded-full bg-gray-300 opacity-60 blur-[20px]'
		},
		{
			icon: Rss,
			bg: 'pointer-events-none absolute left-1/2 top-1/2 size-1/2 -translate-x-1/2 -translate-y-1/2 overflow-visible rounded-full bg-gray-100 opacity-60 blur-[20px]'
		},
		{
			icon: BarChart,
			bg: 'pointer-events-none absolute left-1/2 top-1/2 size-1/2 -translate-x-1/2 -translate-y-1/2 overflow-visible rounded-full bg-gray-200 opacity-60 blur-[20px]'
		}
	];
	function shuffleArray(array: any[]) {
		let currentIndex = array.length;
		let randomIndex;
		// While there remain elements to shuffle.
		while (currentIndex !== 0) {
			// Pick a remaining element.
			randomIndex = Math.floor(Math.random() * currentIndex);
			currentIndex--;
			// And swap it with the current element.
			[array[currentIndex], array[randomIndex]] = [array[randomIndex], array[currentIndex]];
		}
		return array;
	}
	let randomTiles1 = shuffleArray(tiles);
	let randomTiles2 = shuffleArray(tiles);
	let randomTiles3 = shuffleArray(tiles);
	let randomTiles4 = shuffleArray(tiles);
</script>

<section id="cta">
	<div class="py-14 bg-white">
		<div class="flex w-full flex-col items-center justify-center">
			<div class="relative flex w-full flex-col items-center justify-center overflow-hidden">
				<Marquee reverse class="-delay-[200ms] [--duration:10s]" repeat={5}>
					{#each randomTiles1 as { icon, bg }, id}
						<CtaCard class="bg-white border border-gray-200 text-gray-900">
							<svelte:component this={icon} class="size-full text-gray-700" />
							<div class={bg}></div>
						</CtaCard>
					{/each}
				</Marquee>
				<Marquee reverse class="[--duration:25s]" repeat={5}>
					{#each randomTiles2 as { icon, bg }, id}
						<CtaCard class="bg-white border border-gray-200 text-gray-900">
							<svelte:component this={icon} class="size-full text-gray-700" />
							<div class={bg}></div>
						</CtaCard>
					{/each}
				</Marquee>
				<Marquee reverse class="-delay-[200ms] [--duration:20s]" repeat={5}>
					{#each randomTiles1 as { icon, bg }, id}
						<CtaCard class="bg-white border border-gray-200 text-gray-900">
							<svelte:component this={icon} class="size-full text-gray-700" />
							<div class={bg}></div>
						</CtaCard>
					{/each}
				</Marquee>
				<Marquee reverse class="[--duration:30s]" repeat={5}>
					{#each randomTiles2 as { icon, bg }, id}
						<CtaCard class="bg-white border border-gray-200 text-gray-900">
							<svelte:component this={icon} class="size-full text-gray-700" />
							<div class={bg}></div>
						</CtaCard>
					{/each}
				</Marquee>
				<Marquee reverse class="-delay-[200ms] [--duration:20s]" repeat={5}>
					{#each randomTiles3 as { icon, bg }, id}
						<CtaCard class="bg-white border border-gray-200 text-gray-900">
							<svelte:component this={icon} class="size-full text-gray-700" />
							<div class={bg}></div>
						</CtaCard>
					{/each}
				</Marquee>
				<Marquee reverse class="[--duration:30s]" repeat={5}>
					{#each randomTiles4 as { icon, bg }, id}
						<CtaCard class="bg-white border border-gray-200 text-gray-900">
							<svelte:component this={icon} class="size-full text-gray-700" />
							<div class={bg}></div>
						</CtaCard>
					{/each}
				</Marquee>
				<div class="absolute z-10">
					<div
						class="mx-auto size-24 rounded-[2rem] border border-gray-200 bg-white p-3 shadow-2xl backdrop-blur-md lg:size-32"
					>
						<HeartHandshake class="mx-auto size-16 text-black lg:size-24" />
					</div>
					<div class="z-10 mt-4 flex flex-col items-center text-center text-gray-900">
						<h1 class="text-3xl font-bold lg:text-4xl">Want to Contribute ?</h1>
						<p class="mt-2 text-gray-700">Help us and provide your valuable insights</p>
						<Button
							size="lg"
							variant="outline"
							href="https://github.com/LipSync-Edusync"
							class="group mt-4 rounded-[2rem] px-6 border border-gray-300 bg-white text-gray-900"
						>
							github/Lipsync-Edusync
							<ChevronRight
								class="ml-1 size-4 transition-all duration-300 ease-out group-hover:translate-x-1"
							/>
						</Button>
					</div>
					<div
						class="bg-white absolute inset-0 -z-10 rounded-full opacity-30 blur-xl"
					/>
				</div>
				<div
					class="absolute inset-x-0 bottom-0 h-full bg-gradient-to-b from-transparent to-gray-100"
				/>
			</div>
		</div>
	</div>
</section>
