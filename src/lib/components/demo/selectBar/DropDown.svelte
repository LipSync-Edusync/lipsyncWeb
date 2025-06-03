<script lang="ts">
    import { Button } from "$lib/components/ui/button";
    import { clickOutside } from "$lib/actions/clickOutside";

    export let langs: string[];
    export let name: string = 'Button';
    
    let open = false;
    let selected = langs[0];

    function toggle() {
        open = !open;
    }

    function select(lang:string) {
        selected = lang;
        open=false;
    }

    function closeDropdown() {
        open= false;
    }
</script>

<div class="src-button flex justify-center w-48 m-3" use:clickOutside={closeDropdown}>
    <Button 
        class="border p-5 w-full bg-slate-900/55 hover:bg-slate-800/55 transition-all duration-300 hover:scale-[1.01] shadow-lg hover:shadow-slate-600/55 hover:-translate-y-[0.05rem]"
        on:click={toggle}
        >
        <span class="text-white text-sm font-light">{name} : {selected}</span>
    </Button>
    {#if open}
    <div class="lang-cont absolute z-50 w-48 pt-14 flex flex-col self-start opacity-35 animate-fade-in delay-[10ms]">
        {#each langs as lang}
        <Button 
            class="bg-slate-800/55 text-white hover:text-black hover:bg-slate-300"
            on:click={() => select(lang)}
        >
            {lang}
        </Button>
        {/each}
    </div>
    {/if}
</div>