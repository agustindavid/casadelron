<script lang="ts">
	import { onMount, getContext } from 'svelte';
	import gsap from 'gsap';
	import ScrollTrigger from 'gsap/ScrollTrigger';
	import Map from '$lib/components/Map.svelte';
	import { lenisStore } from '$lib/lenisStore';

	let unsubscribe;

	onMount(async () => {
		unsubscribe = lenisStore.subscribe((lenis) => {
			if (lenis) {
				lenis.on('scroll', checkVisibility);
				// Initial check
				checkVisibility();
			}
		});

		// Initial check
		checkVisibility();

		gsap.registerPlugin(ScrollTrigger);

		gsap.to('.hero-image', {
			yPercent: 30,
			ease: 'none',
			scrollTrigger: {
				trigger: '.hero-container',
				start: 'top top',
				end: 'bottom top',
				scrub: true
			}
		});

		// Slide up effect for the overlay (faster scroll)
		gsap.to('.hero-overlay', {
			yPercent: -100,
			ease: 'none',
			scrollTrigger: {
				trigger: '.hero-container',
				start: 'top top',
				end: 'bottom top',
				scrub: true
			}
		});

	
	});

	function checkVisibility() {
		const windowHeight = window.innerHeight;

		document.querySelectorAll('.scroll-reveal').forEach((el) => {
			const rect = el.getBoundingClientRect();
			const isVisible = rect.top < windowHeight * 0.85 && rect.bottom > 0;

			if (isVisible) {
				el.classList.add('visible');
			}
		});
	}

	const items = [
		{ title: 'Item One', text: 'Content here' },
		{ title: 'Item Two', text: 'Content here' },
		{ title: 'Item Three', text: 'Content here' },
		{ title: 'Item Three', text: 'Content here' }
	];
</script>

<svelte:head>
	<title>Casa del Ron México</title>
</svelte:head>

<div class="hero-container relative h-[150vh] overflow-hidden">
	<div class="hero-image absolute top-0 left-0 h-[150vh] w-full overflow-hidden">
		<img
			src="main.jpg"
			alt=""
			class="h-full w-full object-cover object-center will-change-transform"
		/>
	</div>
	<!-- Overlay that slides up -->
	<div
		class="hero-overlay absolute top-0 left-0 z-10 flex
         h-[50vh]
         w-full flex-col items-center justify-center
         bg-black text-orange-50 will-change-transform"
	>
		<img src="logo.png" class="mt-16 w-32" alt="" />
		<!-- Background image that gets revealed -->
		<div class="p-8 text-center">
			<div class=" mb-4 tracking-wider uppercase">Herencia y tradición</div>
			<h1
				class="hero-title mb-8 font-anton text-[clamp(3rem,10vw,7rem)] leading-[0.9] font-black tracking-wide uppercase antialiased select-none"
			>
				El alma del ron mexicano
			</h1>
		</div>
	</div>
</div>

<section class="relative z-20 bg-white lg:px-8 px-4 py-16">
	<div class="mx-auto lg:container">
		<h2 class=" mb-8 text-4xl">De nuestra selección</h2>
		<div class="grid grid-cols-1 gap-8 lg:grid-cols-3">
			<div>
				<a class="block" href="/product">
					<div class="group relative overflow-hidden">
						<img src="1.jpg" alt="" />
						<img
							src="hover.jpg"
							alt=""
							class=" absolute top-0 left-0 z-40 h-full w-full object-cover opacity-0 transition-all group-hover:opacity-100"
						/>
					</div>
				</a>
			</div>
			<div>
				<img src="2.jpg" alt="" />
			</div>
			<div>
				<img src="3.jpg" alt="" />
			</div>
		</div>
	</div>
</section>

<div class="flex lg:h-screen flex-col gap-16 bg-orange-50 lg:pl-5 text-2xl text-black lg:flex-row">
	<div class="lg:w-3/5 w-full lg:order-2">
		<img src="qs.jpg" class="h-full aspect-square lg:aspect-auto object-cover" alt="" />
	</div>

	<div class="flex lg:w-2/5 w-full flex-col items-center pb-16 lg:pb-0 justify-center lg:order-1 px-4">
		<h2 class="mb-8 w-full text-left text-4xl">El ron en México</h2>
		<div class=" font-light">
			<p>
				Lorem ipsum dolor sit, amet consectetur adipisicing elit. Debitis, fugiat ea iusto ut saepe
				maxime placeat quis, quasi quae dolorem eos animi odio iste eum! Iste totam ut vel maxime.
				Quidem, voluptatibus.
			</p>
			<p>
				Lorem ipsum dolor sit amet consectetur, adipisicing elit. Nobis nihil voluptatum quidem,
				numquam inventore accusantium non obcaecati ipsam id corrupti veniam. Nostrum odio ex
				inventore temporibus animi quo dolorem optio! Lorem ipsum dolor sit amet consectetur
				adipisicing elit. Debitis, fugiat ea iusto ut saepe maxime placeat quis, quasi quae dolorem
				eos animi odio iste eum! Iste totam ut vel maxime. Quidem, voluptatibus.
			</p>
		</div>
	</div>
</div>

<section class=" bg-[#112d1e] py-16 text-white">
	<div class="mx-auto lg:px-8">
		<h2 class="mb-8 text-4xl">Orígenes</h2>
		<Map />
	</div>
</section>



<style>
	:global(.scroll-reveal) {
		opacity: 0;
		transform: translateY(80px);
		transition:
			opacity 0.8s ease,
			transform 0.8s ease;
	}

	:global(.scroll-reveal.visible) {
		opacity: 1;
		transform: translateY(0);
	}
</style>
