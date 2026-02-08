<script lang="ts">
	import { onMount } from 'svelte';
	import gsap from 'gsap';
	import ScrollTrigger from 'gsap/ScrollTrigger';
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
</script>

<svelte:head>
	<title>Ron Potosi Melao</title>
</svelte:head>

<div class="hero-container relative h-[150vh] overflow-hidden">
	<div class="hero-image absolute top-0 left-0 h-[150vh] w-full overflow-hidden">
		<img
			src="hero_p.jpg"
			alt=""
			class="h-full w-full object-cover object-center will-change-transform"
		/>
	</div>
	<!-- Overlay that slides up -->
	<div
		class="hero-overlay absolute top-0 left-0 z-10 flex
         w-full flex-col items-center justify-center
         bg-black text-orange-50 will-change-transform"
	>
		<!-- Background image that gets revealed -->
		<div class="px-8 py-20 text-center">
			<div class="mb-4 tracking-wider uppercase">¿Alguien dijo mojitos?</div>
			<h1
				class="hero-title mb-8 font-anton text-[clamp(3rem,10vw,7rem)] leading-[0.9] font-black tracking-wide uppercase antialiased select-none"
			>
				Ron Potosi Melao
			</h1>
		</div>
	</div>
</div>

<section class=" bg-amber-50">
	<div class="mx-auto max-w-3xl py-16 text-center text-4xl">
		<p><span class="italic">Dulce y aromático.</span><br />El sabor de la Huasteca hecho ron.</p>
	</div>
</section>

<section>
	<div class="grid grid-cols-1 gap-8 px-8 py-16 lg:grid-cols-3">
		<div class="col-span-2 grid grid-cols-1 gap-8 lg:grid-cols-2">
			<div>
				<img src="melao.jpg" class="w-full" alt="" />
			</div>
			<div class=" lg:mt-32">
				<img src="melao.jpg" class="w-full" alt="" />
			</div>
			<div class=" lg:-translate-y-32">
				<img src="melao.jpg" class="w-full" alt="" />
			</div>
			<div>
				<img src="melao.jpg" class="w-full" alt="" />
			</div>
		</div>
		<div class="col-span-1">
			<div class="h-full">
				<div class="sticky top-24">
					<p>
						Lorem ipsum dolor sit, amet consectetur adipisicing elit. Sint maxime esse perspiciatis
						natus quisquam exercitationem sit tempore ipsum. Blanditiis modi ipsam amet earum qui?
						Sequi quaerat officia quas reprehenderit vel! Lorem ipsum dolor sit amet consectetur
						adipisicing elit. Sint maxime esse perspiciatis natus quisquam exercitationem sit
						tempore ipsum. Blanditiis modi ipsam amet earum qui? Sequi quaerat officia quas
						reprehenderit vel!
					</p>
					<div class="my-8 flex flex-col gap-4">
						<div class="flex items-center gap-4">
							<p><span class="font-semibold">Origen:</span> Huasteca Potosina, San Luis Potosí.</p>
						</div>
						<div class="flex items-center gap-4">
							<p><span class="font-semibold">Contenido:</span> 1L.</p>
						</div>

						<div class="flex items-center gap-4">
							<p><span class="font-semibold">Uso recomendado:</span> Cocktails.</p>
						</div>
					</div>

					<div>
						<h4 class="font-semibold">Aroma</h4>
						<div class="flex gap-2">
							{#each Array(5) as _, i}
								<div class="h-4 w-12 border {i < 2 ? 'bg-black' : 'bg-transparent'}"></div>
							{/each}
						</div>
					</div>
					<div class="my-8">
						<h4 class="font-semibold">Dulzura</h4>
						<div class="flex gap-2">
							{#each Array(5) as _, i}
								<div class="h-4 w-12 border {i < 4 ? 'bg-black' : 'bg-transparent'}"></div>
							{/each}
						</div>
					</div>
					<div>
						<h4 class="font-semibold">Cuerpo</h4>
						<div class="flex gap-2">
							{#each Array(5) as _, i}
								<div class="h-4 w-12 border {i < 3 ? 'bg-black' : 'bg-transparent'}"></div>
							{/each}
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</section>

<section class="bg-black text-white">
	<div class="py-16 text-center">
		<h3 class="text-4xl">Ideal para:</h3>
	</div>
	<div class="grid items-center lg:h-screen lg:grid-cols-2">
		<div>
			<img src="hover.jpg" class="scroll-reveal aspect-square w-full object-cover" alt="" />
		</div>
		<div class="scroll-reveal flex flex-col gap-4 p-4 lg:p-16">
			<h4 class="border-b border-white pb-4 text-3xl">Mojito</h4>
			<div class="flex flex-col gap-4">
				<h5 class="text-xl">Ingredientes.</h5>
				<ul class="opacity-80">
					<li>4 cl de ron blanco</li>
					<li>3 cl de zumo de lima</li>
					<li>2 cucharaditas de azúcar (blanco o de caña)</li>
					<li>6-10 hojas de hierbabuena</li>
					<li>Agua mineral (agua gasificada)</li>
				</ul>
				<h5 class="text-xl">Preparación.</h5>
				<div class="flex flex-col gap-2 opacity-80">
					<p>
						En un vaso alto, añadir el azúcar, el zumo de lima y las hojas de hierbabuena. Macerar
						suavemente con un mortero para liberar los aceites sin destrozar las hojas.
					</p>
					<p>Incorporar el ron blanco y remover con una cuchara larga para disolver el azúcar.</p>
					<p>Llenar el vaso con abundante hielo picado. Rellenar el resto del vaso con soda.</p>
					<p>Revolver ligeramente y decorar con una rama de hierbabuena y una rodaja de lima.</p>
				</div>
			</div>
		</div>
	</div>
	<div class="grid items-center lg:h-screen lg:grid-cols-2">
		<div class="lg:order-1">
			<img src="hover.jpg" class="scroll-reveal aspect-square w-full object-cover" alt="" />
		</div>
		<div class="scroll-reveal order-0 flex flex-col gap-4 p-4 lg:p-16">
			<h4 class="border-b border-white pb-4 text-3xl">Mojito</h4>
			<div class="flex flex-col gap-4">
				<h5 class="text-xl">Ingredientes.</h5>
				<ul class="opacity-80">
					<li>4 cl de ron blanco</li>
					<li>3 cl de zumo de lima</li>
					<li>2 cucharaditas de azúcar (blanco o de caña)</li>
					<li>6-10 hojas de hierbabuena</li>
					<li>Agua mineral (agua gasificada)</li>
				</ul>
				<h5 class="text-xl">Preparación.</h5>
				<div class="flex flex-col gap-2 opacity-80">
					<p>
						En un vaso alto, añadir el azúcar, el zumo de lima y las hojas de hierbabuena. Macerar
						suavemente con un mortero para liberar los aceites sin destrozar las hojas.
					</p>
					<p>Incorporar el ron blanco y remover con una cuchara larga para disolver el azúcar.</p>
					<p>Llenar el vaso con abundante hielo picado. Rellenar el resto del vaso con soda.</p>
					<p>Revolver ligeramente y decorar con una rama de hierbabuena y una rodaja de lima.</p>
				</div>
			</div>
		</div>
	</div>
	<div class="grid items-center lg:h-screen lg:grid-cols-2">
		<div>
			<img src="hover.jpg" class="scroll-reveal aspect-square w-full object-cover" alt="" />
		</div>
		<div class="scroll-reveal flex flex-col gap-4 p-4 lg:p-16">
			<h4 class="border-b border-white pb-4 text-3xl">Mojito</h4>
			<div class="flex flex-col gap-4">
				<h5 class="text-xl">Ingredientes.</h5>
				<ul class="opacity-80">
					<li>4 cl de ron blanco</li>
					<li>3 cl de zumo de lima</li>
					<li>2 cucharaditas de azúcar (blanco o de caña)</li>
					<li>6-10 hojas de hierbabuena</li>
					<li>Agua mineral (agua gasificada)</li>
				</ul>
				<h5 class="text-xl">Preparación.</h5>
				<div class="flex flex-col gap-2 opacity-80">
					<p>
						En un vaso alto, añadir el azúcar, el zumo de lima y las hojas de hierbabuena. Macerar
						suavemente con un mortero para liberar los aceites sin destrozar las hojas.
					</p>
					<p>Incorporar el ron blanco y remover con una cuchara larga para disolver el azúcar.</p>
					<p>Llenar el vaso con abundante hielo picado. Rellenar el resto del vaso con soda.</p>
					<p>Revolver ligeramente y decorar con una rama de hierbabuena y una rodaja de lima.</p>
				</div>
			</div>
		</div>
	</div>
</section>

<button
	class="before:duration:200 fixed bottom-0 z-50 w-full cursor-pointer border-t border-b border-black bg-black py-4 text-center font-bold text-white uppercase transition-all duration-500 before:absolute before:bottom-0 before:left-0 before:-z-10 before:h-0 before:w-full before:bg-amber-50 before:transition-all hover:text-black hover:before:h-full"
>
	Comprar Ron Potosi Melao
</button>
