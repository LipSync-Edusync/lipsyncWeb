<script lang="ts">
	import Button from '$lib/components/ui/button/button.svelte';
	import { cn } from '$lib/utils';
	import { AlignJustify, XIcon } from 'lucide-svelte';
	import { fly } from 'svelte/transition';
	
	const navItems = [
		{ label: 'Features', href: '/features' },
		{ label: 'Pricing', href: '/pricing' },
		{ label: 'Careers', href: '/careers' },
		{ label: 'Contact Us', href: '/contact' }
	];

	const authItems = [
		{ label: 'Log in', href: '/signin' },
		{ label: 'Sign up', href: '/signup' }
	];

	const menuItem = [
		{
			id: 1,
			label: 'Features',
			href: '#'
		},
		{
			id: 2,
			label: 'Pricing',
			href: '#'
		},
		{
			id: 3,
			label: 'Careers',
			href: '#'
		},
		{
			id: 4,
			label: 'Contact Us',
			href: '#'
		}
	];

	let hamburgerMenuIsOpen = false;

	function toggleOverflowHidden(node: HTMLElement) {
		node.addEventListener('click', () => {
			hamburgerMenuIsOpen = !hamburgerMenuIsOpen;
			const html = document.querySelector('html');
			if (html) {
				if (hamburgerMenuIsOpen) {
					html.classList.add('overflow-hidden');
				} else {
					html.classList.remove('overflow-hidden');
				}
			}
		});
	}
	let innerWidth = 0;
</script>

<svelte:window bind:innerWidth />

<header
    class="fixed left-0 top-0 z-50 w-full -translate-y-4 animate-fade-in border-b border-gray-200 bg-white opacity-0 backdrop-blur-md"
>
    <div class="container flex h-14 items-center justify-between">
        <a class="text-md flex items-center text-gray-900" href="/"> LipSync </a>

        <div class="ml-auto flex h-full items-center">
            <a class="mr-6 text-sm text-gray-700 hover:text-gray-900" href="/signin"> Log in </a>
            <Button variant="secondary" class="mr-6 text-sm bg-white border border-gray-200 text-gray-900 hover:bg-gray-100" href="/signup">Sign up</Button>
        </div>
        <button class="ml-6 md:hidden" use:toggleOverflowHidden>
            <span class="sr-only">Toggle menu</span>
            {#if hamburgerMenuIsOpen}
                <XIcon strokeWidth={1.4} class="text-gray-400" />
            {:else}
                <AlignJustify strokeWidth={1.4} class="text-gray-400" />
            {/if}
        </button>
    </div>
</header>

<nav
    class={cn(
        `fixed left-0 top-0 z-50 h-screen w-full overflow-auto`,
        {
            'pointer-events-none': !hamburgerMenuIsOpen
        },
        {
            'bg-white/90 backdrop-blur-md': hamburgerMenuIsOpen
        }
    )}
>
    {#if hamburgerMenuIsOpen === true}
        <div class="container flex h-14 items-center justify-between">
            <a class="text-md flex items-center text-gray-900" href="/"> LipSync </a>

            <button class="md:hidden" use:toggleOverflowHidden>
                <span class="sr-only">Toggle menu</span>
                {#if hamburgerMenuIsOpen}
                    <XIcon strokeWidth={1.4} class="text-gray-400" />
                {:else}
                    <AlignJustify strokeWidth={1.4} class="text-gray-400" />
                {/if}
            </button>
        </div>
        <ul
            in:fly={{ y: -30, duration: 400 }}
            class="flex flex-col uppercase ease-in md:flex-row md:items-center md:normal-case bg-white"
        >
            {#each menuItem as item, i}
                <li class="border-gray-200 border-b py-0.5 pl-6 md:border-none">
                    <a
                        class="hover:text-gray-900 flex h-[var(--navigation-height)] w-full items-center text-xl text-gray-700 transition-[color,transform] duration-300 md:translate-y-0 md:text-sm md:transition-colors"
                        href={item.href}
                    >
                        {item.label}
                    </a>
                </li>
            {/each}
        </ul>
    {/if}
</nav>