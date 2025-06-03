<script lang="ts">
	import Button from "$lib/components/ui/button/button.svelte";
    import { langs } from '$lib/data/langs';
	import DropDown from "./DropDown.svelte";

</script>

<div class="selectBar justify-center flex flex-row w-full border rounded-lg shadow-lg m-3">
    <DropDown name="Source" langs={langs} />
    <DropDown name="Destination" langs={langs} />
</div>