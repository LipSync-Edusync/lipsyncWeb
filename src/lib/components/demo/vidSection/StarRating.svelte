<script lang="ts">
    export let title: string;

  let rating = 0;
  let hover = 0;

  function setRating(star: number) {
    rating = star;
  }

  function setHover(star: number) {
    hover = star;
  }

  function clearHover() {
    hover = 0;
  }
</script>

<div class="w-fit border rounded-2xl shadow-md text-center p-3 m-2 scale-85 md:scale-105">
  <h2 class="text-sm text-stone-400 font-semibold">{title}</h2>
  
  <div class="flex justify-center gap-2 text-white/90 text-sm scale-105">
    {#each Array(5) as _, i}
      <button
        on:click={() => setRating(i + 1)}
        on:mouseenter={() => setHover(i + 1)}
        on:mouseleave={clearHover}
        class="focus:outline-none transition-transform duration-200 hover:scale-110"
        aria-label="Rate {i + 1} star"
      >
        {@html (hover || rating) > i ? "&#9733;" : "&#9734;"}
      </button>
    {/each}
  </div>

  {#if rating > 0}
    <p class="text-gray-500 text-sm">You rated {rating} out of 5 stars.</p>
  {/if}
</div>
