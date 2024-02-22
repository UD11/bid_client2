<script>
	import { onMount } from 'svelte';
	import { Tabs, TabItem, Avatar } from 'flowbite-svelte';
	import {
		Table,
		TableBody,
		TableBodyCell,
		TableBodyRow,
		TableHead,
		TableHeadCell,
		TableSearch,
		Card
	} from 'flowbite-svelte';
	import { Button, Modal } from 'flowbite-svelte';
	import { ArrowRightOutline } from 'flowbite-svelte-icons';
	import Marquee from 'svelte-fast-marquee';
	// import { API_BASE_URL } from '../../url';

	let scrollingModal = false;
	let searchTerm = '';
	let key = 'all';
	let items = [];
	let team_players = [];
	let Teamdata = {};
	let allteamData = [];
	let filteredTeam = [];

	let teamId = 1;
	let filteredItems = [];
	let records = {};

	async function fetchPlayerData() {
		try {
			// const response = await fetch(`${API_BASE_URL}/getallplayers?key=${key}`);
			const response = await fetch(`api/getallplayers?key=${key}`);
			const data = await response.json();

			items = data;
		} catch (error) {
			console.error('Error fetching data:', error);
		}
	}

	async function fetchRecords() {
		try {
			// const response = await fetch(`${API_BASE_URL}/records`);
			const response = await fetch(`api/records`);
			records = await response.json();
			console.log(records);
		} catch (error) {
			console.log(error);
		}
	}

	// async function fetchTeamData() {
	// 	try {
	// 		const response = await fetch(`http://localhost:8000/api/getteam?team_id=${teamId}`);
	// 		Teamdata = await response.json();

	// 		team_players = Teamdata.players;
	// 	} catch (error) {
	// 		console.error('Error fetching data:', error);
	// 	}
	// }

	async function fetchAllTeamData() {
		try {
			// const resposne = await fetch(`${API_BASE_URL}/getallteam`);
			const resposne = await fetch(`api/getallteam`);
			allteamData = await resposne.json();
		} catch (error) {
			console.log(error);
		}
	}

	function handletab(tabkey) {
		key = tabkey;
		fetchPlayerData();
	}

	onMount(() => {
		fetchPlayerData();
		fetchAllTeamData();
		fetchRecords();
	});

	$: filteredItems = items.filter(
		(item) => item.firstname.toLowerCase().indexOf(searchTerm.toLowerCase()) !== -1
	);

	$: filteredTeam = allteamData.filter((team_data) => team_data.id == teamId);
</script>

<!-- <div class="shadow-lg shadow-cyan-500"> -->
{#if allteamData}
	<Marquee pauseOnHover={true} speed={75} class=" ">
		<TableBodyRow>
			{#each allteamData as teamdata}
				<TableBodyCell class="m-auto text-black">
					<div class="flex flex-col rounded-md p-2 shadow-md shadow-purple-700">
						<div class="flex flex-row space-x-1 font-mono font-bold text-gray-500">
							Team:
							<div class="text-balance font-mono font-bold text-black">
								{teamdata.team_name}
							</div>
						</div>
						<div class="flex items-center justify-between space-x-4 font-mono font-bold">
							<div class="flex flex-row font-mono font-bold text-gray-500">
								purse:
								<div class="font-mono font-bold text-green-500">
									{teamdata.pot}
								</div>
							</div>
							<div class="flex flex-row font-mono font-bold text-gray-500">
								rem:
								<div class="font-mono font-bold text-red-700">
									{teamdata.players.length}/12
								</div>
							</div>
						</div>
					</div>
				</TableBodyCell>
			{/each}
		</TableBodyRow>
	</Marquee>
{/if}
<!-- </div> -->
<div class="min-w-screen mt-2 min-h-screen justify-center bg-white dark:bg-black p-2 md:flex">
	<div class="">
		<div class="flex justify-center">
			<div class="mr-2 flex h-2/4 flex-col justify-center">
				<Card class="h-4/5 w-auto shadow-lg shadow-green-800 dark:bg-black mb-4">
					<h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">
						Current Record Values
					</h5>
					{#if records && records.max_batsman}
						<div class="flex items-center space-x-4 p-2 rtl:space-x-reverse">
							<Avatar src={records.max_batsman.user_image} rounded />
							<div class="min-w-full space-y-1 font-medium text-black dark:text-white">
								<div>{records.max_batsman.firstname + ' ' + records.max_batsman.lastname}</div>
								<div class="flex flex-row space-x-2">
									<div class="text-sm text-gray-500 dark:text-gray-400">
										{records.max_batsman.player_position} | {records.max_batsman.player_value}
									</div>
								</div>
							</div>
						</div>
					{/if}
					{#if records && records.max_bowler}
						<div class="flex items-center space-x-4 p-2 rtl:space-x-reverse">
							<Avatar src={records.max_bowler.user_image} rounded />
							<div class="min-w-full space-y-1 font-medium text-black dark:text-white">
								<div>{records.max_bowler.firstname + ' ' + records.max_bowler.lastname}</div>
								<div class="flex flex-row space-x-2">
									<div class="text-sm text-gray-500 dark:text-gray-400">
										{records.max_bowler.player_position} | {records.max_bowler.player_value}
									</div>
								</div>
							</div>
						</div>
					{/if}
					{#if records && records.max_wk}
						<div class="flex items-center space-x-4 p-2 rtl:space-x-reverse">
							<Avatar src={records.max_wk.user_image} rounded />
							<div class="min-w-full space-y-1 font-medium text-black dark:text-white">
								<div>{records.max_wk.firstname + ' ' + records.max_wk.lastname}</div>
								<div class="flex flex-row space-x-2">
									<div class="text-sm text-gray-500 dark:text-gray-400">
										{records.max_wk.player_position} | {records.max_bowler.player_value}
									</div>
								</div>
							</div>
						</div>
					{/if}
					{#if records && records.max_bla}
						<div class="flex items-center space-x-4 p-2 rtl:space-x-reverse">
							<Avatar src={records.max_bla.user_image} rounded />
							<div class="min-w-full space-y-1 font-medium text-black dark:text-white">
								<div>{records.max_bla.firstname + ' ' + records.max_bla.lastname}</div>
								<div class="flex flex-row space-x-2">
									<div class="text-sm text-gray-500 dark:text-gray-400">
										{records.max_bla.player_position} | {records.max_bowler.player_value}
									</div>
								</div>
							</div>
						</div>
					{/if}
					{#if records && records.max_bta}
						<div class="flex items-center space-x-4 p-2 rtl:space-x-reverse">
							<Avatar src={records.max_bta.user_image} rounded />
							<div class="min-w-full space-y-1 font-medium text-black dark:text-white">
								<div>{records.max_bta.firstname + ' ' + records.max_bta.lastname}</div>
								<div class="flex flex-row space-x-2">
									<div class="text-sm text-gray-500 dark:text-gray-400">
										{records.max_bta.player_position} | {records.max_bowler.player_value}
									</div>
								</div>
							</div>
						</div>
					{/if}
					
					<Button class="mt-2"><a href="/">Register as a Player</a></Button>
				</Card>
			</div>
		</div>
	</div>

	<div class="md:w-4/5 shadow-lg shadow-cyan-600">
		<Tabs style="underline" class=" shadow-md shadow-cyan-600">
			<TabItem open on:click={() => handletab('all')} title="All Players">
				<!-- <div class="w-5/6 "> -->
				<TableSearch
					placeholder="Search by player name"
					hoverable={true}
					bind:inputValue={searchTerm}
				>
					<Table hoverable={true} striped={true}>
						<TableHead>
							<TableHeadCell></TableHeadCell>
							<TableHeadCell>Name</TableHeadCell>
							<TableHeadCell>Playing Role</TableHeadCell>
							<TableHeadCell>Year</TableHeadCell>
							<TableHeadCell>Department</TableHeadCell>
							<TableHeadCell>Player Value</TableHeadCell>
							<TableHeadCell>Status</TableHeadCell>
						</TableHead>
						<TableBody class="divide-y ">
							{#each filteredItems as item}
								<TableBodyRow>
									<TableBodyCell>
										<Avatar src={item.user_image} alt="alt" class="flex-shrink-0" />
									</TableBodyCell>
									<TableBodyCell>{item.firstname + ' ' + item.lastname}</TableBodyCell>
									<TableBodyCell>{item.player_position}</TableBodyCell>
									<TableBodyCell>{item.year}</TableBodyCell>
									<TableBodyCell>{item.department}</TableBodyCell>
									<TableBodyCell>{item.player_value}</TableBodyCell>
									<TableBodyCell>{item.sold === true ? 'Sold' : 'Unsold'}</TableBodyCell>
								</TableBodyRow>
							{/each}
						</TableBody>
					</Table>
				</TableSearch>
				<!-- </div> -->
			</TabItem>
			<TabItem on:click={() => handletab('sold')} title="Sold Players">
				<!-- <div class="w-5/6"> -->
				<TableSearch
					placeholder="Search by player name"
					hoverable={true}
					bind:inputValue={searchTerm}
				>
					<Table hoverable={true} striped={true}>
						<TableHead>
							<TableHeadCell></TableHeadCell>
							<TableHeadCell>Name</TableHeadCell>
							<TableHeadCell>Playing Role</TableHeadCell>
							<TableHeadCell>Year</TableHeadCell>
							<TableHeadCell>Department</TableHeadCell>
							<TableHeadCell>Player Value</TableHeadCell>
							<TableHeadCell>Status</TableHeadCell>
						</TableHead>
						<TableBody class="divide-y">
							{#each filteredItems as item}
								<TableBodyRow>
									<TableBodyCell>
										<Avatar src={item.user_image} alt="alt" class="flex-shrink-0" />
									</TableBodyCell>
									<TableBodyCell>{item.firstname}</TableBodyCell>
									<TableBodyCell>{item.player_position}</TableBodyCell>
									<TableBodyCell>{item.year}</TableBodyCell>
									<TableBodyCell>{item.department}</TableBodyCell>
									<TableBodyCell>{item.player_value}</TableBodyCell>
									<TableBodyCell>Sold</TableBodyCell>
								</TableBodyRow>
							{/each}
						</TableBody>
					</Table>
				</TableSearch>
				<!-- </div> -->
			</TabItem>
			<TabItem on:click={() => handletab('unsold')} title="Unsold Players">
				<!-- <div class="w-5/6"> -->
				<TableSearch
					placeholder="Search by player name"
					hoverable={true}
					bind:inputValue={searchTerm}
				>
					<Table hoverable={true} striped={true}>
						<TableHead>
							<TableHeadCell></TableHeadCell>
							<TableHeadCell>Name</TableHeadCell>
							<TableHeadCell>Playing Role</TableHeadCell>
							<TableHeadCell>Year</TableHeadCell>
							<TableHeadCell>Department</TableHeadCell>
							<TableHeadCell>Player Value</TableHeadCell>
							<TableHeadCell>Status</TableHeadCell>
						</TableHead>
						<TableBody class="divide-y">
							{#each filteredItems as item}
								<TableBodyRow>
									<TableBodyCell>
										<Avatar src={item.user_image} alt="alt" class="flex-shrink-0" />
									</TableBodyCell>
									<TableBodyCell>{item.firstname}</TableBodyCell>
									<TableBodyCell>{item.player_position}</TableBodyCell>
									<TableBodyCell>{item.year}</TableBodyCell>
									<TableBodyCell>{item.department}</TableBodyCell>
									<TableBodyCell>{item.player_value}</TableBodyCell>
									<TableBodyCell>Unsold</TableBodyCell>
								</TableBodyRow>
							{/each}
						</TableBody>
					</Table>
				</TableSearch>
				<!-- </div> -->
			</TabItem>

			<TabItem title="Teams">
				<Table hoverable={true} striped={true}>
					<TableHead>
						<TableHeadCell>Team Name</TableHeadCell>
						<TableHeadCell>Owner</TableHeadCell>
						<TableHeadCell>Co-Owner</TableHeadCell>
						<TableHeadCell>Captain</TableHeadCell>
						<TableHeadCell>Vice-Captain</TableHeadCell>
						<TableHeadCell>Star Player</TableHeadCell>
						<TableHeadCell>Purse Value</TableHeadCell>
						<TableHeadCell>Players</TableHeadCell>
						<TableHeadCell>
							<span class="sr-only">Edit</span>
						</TableHeadCell>
					</TableHead>
					<TableBody class="divide-y">
						{#each allteamData as team_data}
							<TableBodyRow>
								<TableBodyCell>{team_data.team_name}</TableBodyCell>
								<TableBodyCell>{team_data.owner.firstname}</TableBodyCell>
								<TableBodyCell>{team_data.coowner.firstname}</TableBodyCell>
								<TableBodyCell>{team_data.captain.firstname}</TableBodyCell>
								<TableBodyCell>{team_data.vicecaptain.firstname}</TableBodyCell>
								<TableBodyCell>{team_data.marquee.firstname}</TableBodyCell>
								<TableBodyCell>{team_data.pot}</TableBodyCell>
								<TableBodyCell>
									<!-- <Button on:click={() => (scrollingModal = true)} autoclose>Scrolling modal</Button -->
									<Button
										on:click={() => ((scrollingModal = true), (teamId = team_data.id))}
										autoclose
										pill={true}
										class="!p-2"
										size="xs"
										outline
										color="green"><ArrowRightOutline class="h-4 w-4" /></Button
									>
									<Modal title="List of players" bind:open={scrollingModal}>
										<Table>
											<TableHead>
												<TableHeadCell>Name</TableHeadCell>
												<TableHeadCell>Playing Role</TableHeadCell>
												<TableHeadCell>Year</TableHeadCell>
												<TableHeadCell>Department</TableHeadCell>
												<TableHeadCell>Player Value</TableHeadCell>
											</TableHead>
											<TableBody class="divide-y">
												{#each filteredTeam[0].players as player}
													<TableBodyRow>
														<TableBodyCell>{player.username}</TableBodyCell>
														<TableBodyCell>{player.player_position}</TableBodyCell>
														<TableBodyCell>{player.year}</TableBodyCell>
														<TableBodyCell>{player.department}</TableBodyCell>
														<TableBodyCell>{player.player_value}</TableBodyCell>
													</TableBodyRow>
												{/each}
											</TableBody>
										</Table>
									</Modal>
								</TableBodyCell>
							</TableBodyRow>
						{/each}
					</TableBody>
				</Table>
			</TabItem>
		</Tabs>
	</div>
</div>
