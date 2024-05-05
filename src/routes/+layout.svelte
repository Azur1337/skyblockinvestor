<script lang="ts">
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';
	import Themecontroller from '../components/Themecontroller.svelte';
	import BurgerIcon from '../components/icons/BurgerIcon.svelte';
	import DashboardIcon from '../components/icons/DashboardIcon.svelte';
	import InvestIcon from '../components/icons/InvestIcon.svelte';
	import StatIcon from '../components/icons/StatIcon.svelte';
	import AchievementIcon from '../components/icons/AchievementIcon.svelte';
	let theme: any;
	onMount(() => {
		theme = localStorage.getItem('theme');
	});
</script>

<div class="drawer lg:drawer-open">
	<input id="my-drawer-2" type="checkbox" class="drawer-toggle" />
	<div class="drawer-content flex flex-col items-center justify-center">
		<slot />
		<label for="my-drawer-2" class="btn drawer-button lg:hidden mr-auto ml-2 mt-2">
			<BurgerIcon />
		</label>
	</div>
	<div class="drawer-side">
		<label for="my-drawer-2" aria-label="close sidebar" class="drawer-overlay"></label>
		<ul class="menu p-4 w-80 min-h-full bg-base-200 text-base-content">
			<!-- Sidebar content here -->
			<Themecontroller />
			<li on:click={() => goto('/Dashboard')}>
				<a class="text-xl"><DashboardIcon /> Dashboard</a>
			</li>
			<li on:click={() => goto('/Invests')}><a class="text-xl"><InvestIcon /> Invests </a></li>
			<li on:click={() => goto('/Statistics')}><a class="text-xl"><StatIcon /> Statistics </a></li>
			<li on:click={() => goto('/Achievements')}>
				<a class="text-xl"><AchievementIcon /> Achievements </a>
			</li>
		</ul>
	</div>
</div>
