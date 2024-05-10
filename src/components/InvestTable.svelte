<script>
	import ModalStats from './ModalStats.svelte';
</script>

<div class="overflow-x-auto">
	<table class="table">
		<!-- head -->
		<thead>
			<tr>
				<th>Name</th>
				<th>Quantity</th>
				<th>Profit Per Stock</th>
				<th></th>
			</tr>
		</thead>
		<tbody>
			<!-- row 1 -->
			<tr>
				<td>
					<div class="flex items-center gap-3">
						<div class="avatar">
							<div class="mask mask-squircle w-12 h-12">
								<img src="src/components/images/BlackSheep.png" alt="Black Sheep Pet Skin" />
							</div>
						</div>
						<div>
							<div class="font-bold">Black Sheep Skin</div>
							<div class="text-sm opacity-50">Pet Skin</div>
						</div>
					</div>
				</td>
				<td>
					<span class="badge badge-ghost badge-sm">1</span>
				</td>
				<td class="text-green-500 font-bold">+1 130 932 102$</td>
				<th>
					<button class="btn btn-ghost btn-xs">details</button>
				</th>
			</tr>
			<!-- row 2 -->
			<tr>
				<td>
					<div class="flex items-center gap-3">
						<div class="avatar">
							<div class="mask mask-squircle w-12 h-12">
								<img src="src/components/images/PurpleSheep.png" alt="Purple Sheep Pet Skin" />
							</div>
						</div>
						<div>
							<div class="font-bold">Purple Sheep Skin</div>
							<div class="text-sm opacity-50">Pet Skin</div>
						</div>
					</div>
				</td>
				<td>
					<span class="badge badge-ghost badge-sm">2</span>
				</td>
				<td class="text-green-500 font-bold">+529 725 082$</td>
				<th>
					<button class="btn btn-ghost btn-xs">details</button>
				</th>
			</tr>
			<!-- row 3 -->
			<tr>
				<td>
					<div class="flex items-center gap-3">
						<div class="avatar">
							<div class="mask mask-squircle w-12 h-12">
								<img
									src="src/components/images/WitherGoggles.png"
									alt="Celestial Wither Goggles Skin"
								/>
							</div>
						</div>
						<div>
							<div class="font-bold">Celestial Wither Goggles Skin</div>
							<div class="text-sm opacity-50">Helmet Skin</div>
						</div>
					</div>
				</td>
				<td>
					<span class="badge badge-ghost badge-sm">4</span>
				</td>
				<td class="text-green-500 font-bold">+30 727 092$</td>
				<th>
					<button class="btn btn-ghost btn-xs">details</button>
				</th>
			</tr>
			<!-- row 4 -->
			<tr>
				<td>
					<div class="flex items-center gap-3">
						<div class="avatar">
							<div class="mask mask-squircle w-12 h-12">
								<img src="src/components/images/MeowMusicRune.png" alt="Meow Music Rune" />
							</div>
						</div>
						<div>
							<div class="font-bold">Meow Music Rune</div>
							<div class="text-sm opacity-50">Weapon Rune</div>
						</div>
					</div>
				</td>
				<td>
					<span class="badge badge-ghost badge-sm">69</span>
				</td>
				<td class="text-green-500 font-bold">+1 932 102$</td>
				<th>
					<button class="btn btn-ghost btn-xs" on:click={() => modal.showModal()}>details</button>
				</th>
			</tr>
		</tbody>
	</table>
</div>

<dialog id="modal" class="modal">
	<div class="modal-box">
		<form method="dialog">
			<button class="btn btn-sm btn-circle btn-ghost absolute right-2 top-2">✕</button>
		</form>
		<h3 class="font-bold text-lg">Meow Music Rune</h3>
		<ModalStats />
	</div>
</dialog>
