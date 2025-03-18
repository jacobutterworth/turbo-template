<script lang="ts">
	import '../app.css';
	import DivButton from '$components/DivButton.svelte';
	import { page } from '$app/state';
	const { children } = $props();

	const navItems = [
		{ href: '/', label: 'Home' },
		{ href: '/docs', label: 'Documentation' },
		{ href: '/examples', label: 'Examples' },
		{ href: '/about', label: 'About' }
	];

	let isMobileMenuOpen = $state(false);

	let isActive = $state((href: string) => {
		const currentPath = page.url.pathname;
		return href === '/' ? currentPath === '/' : currentPath.startsWith(href);
	});

	$effect(() => {
		const currentPath = page.url.pathname;
		isActive = (href: string) => {
			return href === '/' ? currentPath === '/' : currentPath.startsWith(href);
		};
	});
</script>

<div class="flex min-h-screen">
	<!-- Sidebar for desktop -->
	<aside class="hidden md:fixed md:inset-y-0 md:flex md:w-64 md:flex-col">
		<div class="flex min-h-0 flex-1 flex-col bg-gray-600">
			<div class="flex flex-1 flex-col overflow-y-auto pt-5 pb-4">
				<div class="flex flex-shrink-0 items-center px-4">
					<h1 class="text-2xl font-bold text-white">Your App</h1>
				</div>
				<nav class="mt-5 flex-1 space-y-1 px-2">
					{#each navItems as item}
						<a
							href={item.href}
							class="group flex items-center rounded-md px-2 py-2 text-sm font-medium {isActive(
								item.href
							)
								? 'bg-gray-700 text-white'
								: 'text-gray-300 hover:bg-gray-700 hover:text-white'}"
							aria-current={isActive(item.href) ? 'page' : undefined}
						>
							{item.label}
						</a>
					{/each}
				</nav>
			</div>
		</div>
	</aside>

	<!-- Mobile menu button -->
	<div class="fixed top-0 left-0 z-20 p-4 md:hidden">
		<button
			onclick={() => (isMobileMenuOpen = !isMobileMenuOpen)}
			class="text-gray-500 hover:text-gray-600 focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 focus:outline-none"
		>
			<span class="sr-only">Open sidebar</span>
			<!-- Heroicon menu icon -->
			<svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" aria-hidden="true">
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M4 6h16M4 12h16M4 18h16"
				/>
			</svg>
		</button>
	</div>

	<!-- Mobile menu -->
	{#if isMobileMenuOpen}
		<div class="fixed inset-0 z-40 flex md:hidden">
			<DivButton
				class="bg-opacity-75 fixed inset-0 bg-gray-600"
				onClick={() => (isMobileMenuOpen = false)}
			/>
			<div class="relative flex w-full max-w-xs flex-1 flex-col bg-gray-800">
				<div class="absolute top-0 right-0 -mr-12 pt-2">
					<button
						onclick={() => (isMobileMenuOpen = false)}
						class="ml-1 flex h-10 w-10 items-center justify-center rounded-full focus:ring-2 focus:ring-white focus:outline-none focus:ring-inset"
					>
						<span class="sr-only">Close sidebar</span>
						<!-- Heroicon x icon -->
						<svg
							class="h-6 w-6 text-white"
							fill="none"
							viewBox="0 0 24 24"
							stroke="currentColor"
							aria-hidden="true"
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M6 18L18 6M6 6l12 12"
							/>
						</svg>
					</button>
				</div>
				<div class="h-0 flex-1 overflow-y-auto pt-5 pb-4">
					<div class="flex flex-shrink-0 items-center px-4">
						<h1 class="text-2xl font-bold text-white">Your App</h1>
					</div>
					<nav class="mt-5 space-y-1 px-2">
						{#each navItems as item}
							<a
								href={item.href}
								class="group flex items-center rounded-md px-2 py-2 text-base font-medium {isActive(
									item.href
								)
									? 'bg-gray-700 text-white'
									: 'text-gray-300 hover:bg-gray-700 hover:text-white'}"
								aria-current={isActive(item.href) ? 'page' : undefined}
							>
								{item.label}
							</a>
						{/each}
					</nav>
				</div>
			</div>
		</div>
	{/if}

	<!-- Main content -->
	<div class="flex flex-1 flex-col md:pl-64">
		<main class="flex-1 p-4">
			{@render children()}
		</main>
	</div>
</div>
