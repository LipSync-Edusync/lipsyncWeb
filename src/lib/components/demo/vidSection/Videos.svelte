<script lang="ts">
	import v1 from '$lib/data/videos/src/v1.webm';
    import { reviews } from '$lib/data/reviews';
	import StarRating from './StarRating.svelte';

    let isVid = true;

</script>

<div class="selectBar justify-center flex flex-row w-full border rounded-lg shadow-lg m-3 p-3">
    {#if isVid}
        <div class="cont flex flex-col">
            <span class="text-xl bg-gradient-to-t from-slate-600 to-slate-100 text-transparent bg-clip-text mb-5">
                Demo Videos Fetched
            </span>
            <div class="video-container flex flex-row gap-3">
                <div class="src border">
                    <video src={v1} controls>
                        <track kind="captions" src="" srclang="en" label="English captions" default />
                    </video>
                </div>
                <div class="dest border">
                    <video src={v1} controls>
                        <track kind="captions" src="" srclang="en" label="English captions" default />
                    </video>
                </div>
            </div>
            <div class="rev-cont flex flex-col justify-center md:flex-row border rounded-lg">
                {#each reviews as review}
                    <StarRating title={review} />
                {/each}
            </div>
        </div>
    {:else}
        <span class="text-xl bg-gradient-to-t from-slate-600 to-slate-100 text-transparent bg-clip-text">
            Select a Language Pair
        </span>
    {/if}
</div>