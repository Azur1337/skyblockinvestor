<script>
	import MoneyIcon from './icons/MoneyIcon.svelte';
</script>

<div class="stats shadow bg-base-200">
	<div class="stat">
		<div class="stat-figure text-red-500 text-5xl">
			<MoneyIcon />
		</div>
		<div class="stat-title">Current Profit/Loss</div>
		<div class="stat-value text-red-500">-25.6K</div>
		<div class="stat-desc">21% more than last month</div>
	</div>

	<div class="stat">
		<div class="stat-figure text-secondary">
			<div class="w-16 rounded-full">
				<img src="src/components/images/BabySuperiorDragon.png" />
			</div>
		</div>
		<div class="stat-title">Page Views</div>
		<div class="stat-value text-green-500">+2.6M</div>
		<div class="stat-desc">1 in Stock</div>
	</div>

	<div class="stat">
		<div class="stat-figure text-secondary">
			<div class="w-16 rounded-full">
				<img src="src/components/images/MeowMusicRune.png" />
			</div>
		</div>
		<div class="stat-title">Meow Music Rune</div>
		<div class="stat-value text-green-500">+10.3M</div>
		<div class="stat-desc">31 in Stock</div>
	</div>
</div>
