<script>
	import DiscordSvg from '$lib/imgs/discord.svg';
	import TwitterSvg from '$lib/imgs/x.svg';

	const footerNavs = [
		{
			label: 'Product',
			items: [
				{
					href: '/',
					name: 'Email Collection'
				},
				{
					href: '/', // /pricing
					name: 'Pricing'
				},
				{
					href: '/', // /faq
					name: 'FAQ'
				}
			]
		},

		{
			label: 'Community',
			items: [
				{
					href: '/',
					name: 'Discord'
				},
				{
					href: '/',
					name: 'Twitter'
				},
				{
					href: 'mailto:tahmidchoudhury019@gmail.com',
					name: 'Email'
				}
			]
		},
		{
			label: 'Legal',
			items: [
				{
					href: '', // /termss
					name: 'Terms'
				},

				{
					href: '', // .privacy
					name: 'Privacy'
				}
			]
		}
	];

	const footerSocials = [
		{
			href: '',
			name: 'Discord',
			icon: DiscordSvg
		}
	];
</script>

<footer>
    <div class="mx-auto w-full max-w-screen-xl xl:pb-2">
        <div class="gap-4 p-4 px-8 py-16 sm:pb-16 md:flex md:justify-between bg-white">
            <div class="mb-12 flex flex-col gap-4">
                <a href="#" class="flex items-center gap-2">
                    <svg
                        width="50"
                        height="50"
                        viewBox="0 0 50 50"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                        xmlns:xlink="http://www.w3.org/1999/xlink"
                    >
                        <!-- ...existing SVG code... -->
                    </svg>
                    <span class="self-center whitespace-nowrap text-2xl font-semibold text-gray-900">
                        LipSync
                    </span>
                </a>
                <p class="max-w-xs text-gray-700">Multilingua Video Translation Framework</p>
            </div>
            <div class="grid grid-cols-1 gap-8 sm:grid-cols-3 sm:gap-10">
                {#each footerNavs as nav}
                    <div>
                        <h2
                            class="mb-6 text-sm font-medium uppercase tracking-tighter text-gray-900"
                        >
                            {nav.label}
                        </h2>
                        <ul class="grid gap-2">
                            {#each nav.items as item}
                                <li>
                                    <a
                                        href={item.href}
                                        class="cursor-pointer text-sm font-[450] text-gray-500 duration-200 hover:text-gray-900"
                                    >
                                        {item.name}
                                    </a>
                                </li>
                            {/each}
                        </ul>
                    </div>
                {/each}
            </div>
        </div>

        <div
            class="flex flex-col gap-2 rounded-md border border-gray-200 bg-white px-8 py-4 sm:flex sm:flex-row sm:items-center sm:justify-between"
        >
            <div class="flex items-center space-x-5 sm:mt-0 sm:justify-center">
                {#each footerSocials as social}
                    <a
                        href={social.href}
                        class="fill-gray-400 text-gray-400 hover:fill-gray-900 hover:text-gray-900"
                    >
                        <img src={social.icon} class="size-4" alt={social.name} />
                        <span class="sr-only">{social.name}</span>
                    </a>
                {/each}
            </div>
            <span class="text-sm text-gray-500 sm:text-center">
                Built for Educational and Research purpose only
            </span>
        </div>
    </div>
</footer>