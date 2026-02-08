<script lang="ts">
	import './layout.css';
	import favicon from '$lib/assets/favicon.svg';
	import Lenis from 'lenis';
	import { onMount, setContext } from 'svelte';
	import gsap from 'gsap';
	import ScrollTrigger from 'gsap/ScrollTrigger';
	import { fade } from 'svelte/transition';
	import { states } from '$lib/constants';
	import { lenisStore } from '$lib/lenisStore';

	let { children } = $props();

	let scrolledDistance = $state(0);

	let openJumbotron = $state(false);
	let lenis = $state<Lenis | null>(null);

	let pathName = $state('');

	let isHome = $state(false);

	onMount(async () => {
		pathName = window.location.pathname;

		if (pathName == '/') {
			isHome = true;
		}

		// Register ScrollTrigger plugin
		gsap.registerPlugin(ScrollTrigger);

		const lenis = new Lenis({
			duration: 1.2,
			easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
			orientation: 'vertical',
			smoothWheel: true,
			wheelMultiplier: 1,
			touchMultiplier: 2
		});

		lenisStore.set(lenis);

		// Synchronize Lenis scrolling with GSAP's ScrollTrigger plugin
		lenis.on('scroll', ScrollTrigger.update);

		lenis.on('scroll', () => {
			scrolledDistance = lenis.scroll;
		});

		// Add Lenis's requestAnimationFrame (raf) method to GSAP's ticker
		gsap.ticker.add((time: number) => {
			lenis.raf(time * 1000);
		});
	});

	setContext('lenis', {
		getLenis: () => lenis
	});
</script>

<svelte:head><link rel="icon" href={favicon} /></svelte:head>

<header
	class="fixed top-0 right-0 left-0 z-50 flex items-center justify-between transition-all {(scrolledDistance <
		400 &&
		!openJumbotron) ||
	!isHome
		? ' text-white'
		: ' text-black'} bg-black px-8 py-4 text-white"
>
	<div>
		<ul>
			<li>
				<button class=" cursor-pointer uppercase" onclick={() => (openJumbotron = !openJumbotron)}
					>Tienda</button
				>
			</li>
			{#if openJumbotron}
				<div transition:fade class="absolute top-18 left-0 h-auto w-full bg-black px-8 py-4 pb-16">
					<div class="grid grid-cols-2 gap-4 lg:container lg:grid-cols-6">
						<div>
							<h2 class="mb-4 text-lg underline px-4 lg:px-0">Orígenes</h2>
							<ul>
								{#each states as state}
									<li>
										<a class="opacity-80 transition-all hover:opacity-100" href="/region/{state.id}"
											>{state.name}</a
										>
									</li>
								{/each}
							</ul>
						</div>
						<div>
							<h2 class="mb-4 text-lg underline">Destilerias</h2>
							<ul>
								<li><a class="opacity-80 transition-all hover:opacity-100">Ron Potosi</a></li>
								<li><a class="opacity-80 transition-all hover:opacity-100">Ron Prohibido</a></li>
								<li><a class="opacity-80 transition-all hover:opacity-100">Ron de ejemplo</a></li>
								<li><a class="opacity-80 transition-all hover:opacity-100">Otro Ron</a></li>
							</ul>
						</div>
						<div>
							<h2 class="mb-4 text-lg underline">Clase</h2>
							<ul>
								<li><a class="opacity-80 transition-all hover:opacity-100">Ron blanco</a></li>
								<li><a class="opacity-80 transition-all hover:opacity-100">Ron dorado</a></li>
								<li><a class="opacity-80 transition-all hover:opacity-100">Ron añejo</a></li>
								<li><a class="opacity-80 transition-all hover:opacity-100">Ron Premium</a></li>
							</ul>
						</div>
						<div class="col-span-1">
							<h2>Nuestra Recomendación</h2>
							<div class="p-4">
								<img src="hover.jpg" class="aspect-square w-full object-cover" alt="" />
							</div>
						</div>
					</div>
				</div>
			{/if}
		</ul>
	</div>
	{#if scrolledDistance > 400 || openJumbotron || pathName != '/'}
		<div transition:fade><a href="/"><img src="isotipo.png" class="w-8" alt="" /></a></div>
	{/if}
	<div></div>
</header>

{@render children()}

<footer class=" w-full bg-black py-32 text-center text-white">
	<p>© 2026 Casa del Ron México</p>
</footer>
