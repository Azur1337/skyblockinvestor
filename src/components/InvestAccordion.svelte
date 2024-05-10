<script>
	import InvestStats from './InvestStats.svelte';
</script>

<div class="join join-vertical w-3/4">
	<div class="collapse collapse-arrow join-item border border-base-300">
		<input type="radio" name="my-accordion-4" checked="checked" />
		<div class="collapse-title text-xl font-medium flex items-center">
			<img src="src/components/images/MeowMusicRune.png" alt="Meow Music Rune" class="w-12" />
			<p class="ml-2">Meow Music Rune</p>
		</div>
		<div class="collapse-content">
			<InvestStats />
		</div>
	</div>
	<div class="collapse collapse-arrow join-item border border-base-300">
		<input type="radio" name="my-accordion-4" checked="checked" />
		<div class="collapse-title text-xl font-medium flex items-center">
			<img src="src/components/images/BlackSheep.png" alt="Meow Music Rune" class="w-12" />
			<p class="ml-2">Black Sheep Skin</p>
		</div>
		<div class="collapse-content">
			<p>hello</p>
		</div>
	</div>
	<div class="collapse collapse-arrow join-item border border-base-300">
		<input type="radio" name="my-accordion-4" checked="checked" />
		<div class="collapse-title text-xl font-medium flex items-center">
			<img src="src/components/images/PurpleSheep.png" alt="Meow Music Rune" class="w-12" />
			<p class="ml-2">Purple Sheep Skin</p>
		</div>
		<div class="collapse-content">
			<p>hello</p>
		</div>
	</div>
</div>
