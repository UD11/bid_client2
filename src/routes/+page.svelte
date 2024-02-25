<script>
	import {
		Input,
		Label,
		Helper,
		Button,
		Checkbox,
		Select,
		Modal,
		Spinner,
		Toast,
		Fileupload,
		Footer,
		FooterCopyright,
		GradientButton
	} from 'flowbite-svelte';
	import { ExclamationCircleSolid } from 'flowbite-svelte-icons';
	import axios from 'axios';

	import { Carousel, Thumbnails } from 'flowbite-svelte';
	import image from '../assets/bg.jpg';
	import image1 from '../assets/bg2.jpg';
	import image2 from '../assets/bg3.jpg';
	import image3 from '../assets/bg1.jpeg';

	let index = 0;
	let forward = true;

	let defaultModal = false;
	let isplayer = false;
	let isloading = false;
	let isregistered = false;

	let images = [
		{ src: image, alt: 'Image 1' },
		{ src: image1, alt: 'Image 2' },
		{ src: image2, alt: 'Image 3' },
		{ src: image3, alt: 'Image 4' }
		// Add more images as needed
	];

	let formdata = {
		firstname: '',
		lastname: '',
		email: '',
		username: '',
		year: '',
		password: 'default',
		file: null,
		department: '',
		playerposition: 'NA'
	};

	let role_list = [
		{ value: 'Batsman', name: 'Batsman' },
		{ value: 'Wicketkeeper', name: 'Wicket-keeper' },
		{ value: 'Bowler', name: 'Bowler' },
		{ value: 'Bowling-All-Rounder', name: 'Bowling All-Rounder' },
		{ value: 'Batting-All-Rounder', name: 'Batting All-Rounder' }
	];
	let year_list = [
		{ value: '1st', name: '1st' },
		{ value: '2nd', name: '2nd' },
		{ value: '3rd', name: '3rd' },
		{ value: '4th', name: '4th' },
		{ value: 'Passout', name: 'passout' }
	];

	let department_list = [
		{ value: 'cse-aiml', name: 'CSE-AIML' },
		{ value: 'cse', name: 'CSE' },
		{ value: 'cse-iot', name: 'CSE-IOT' },
		{ value: 'cst', name: 'CST' },
		{ value: 'csit', name: 'CSIT' },
		{ value: 'ece', name: 'ECE' },
		{ value: 'ee', name: 'EE' },
		{ value: 'me', name: 'ME' },
		{ value: 'biotech', name: 'biotech' },
		{ value: 'bba', name: 'BBA' },
		{ value: 'bca', name: 'BCA' },
		{ value: 'bba-llb', name: 'BBA LLB (HONS)' },
		{ value: 'bhm', name: 'BHM' },
		{ value: 'mba', name: 'MBA' },
		{ value: 'mca', name: 'MCA' }
	];

	let okRegister = false;
	let fileSizeLimitMB = 10;
	let showerrortoast = false;
	let toast_message = `some error occurred`;
	let site_shutdown = false;

	const handlefilechange = async (event) => {
		const fileInput = event.target;
		const selectedFile = fileInput.files[0];

		if (selectedFile) {
			const fileSize = selectedFile.size / 1024 / 1024;

			if (fileSize > fileSizeLimitMB) {
				console.error(`File size exceeds the limit of ${fileSizeLimitMB}MB`);
				okRegister = false;
				toast_message = `File size exceeds limit of ${fileSizeLimitMB}MB`;
				showerrortoast = true;
			} else {
				okRegister = true;
			}

			formdata.file = selectedFile;
			console.log('Selected file:', selectedFile.name);
			console.log('File size:', fileSize.toFixed(2), 'MB');
		}
	};

	const handlesubmit = async (event) => {
		event.preventDefault();
		isloading = true;
		const formData = new FormData();
		formData.append('firstname', formdata.firstname);
		formData.append('lastname', formdata.lastname);
		formData.append('email', formdata.email);
		formData.append('username', formdata.username);
		formData.append('password', formdata.password);
		formData.append('year', formdata.year);
		formData.append('user_image', formdata.file);
		formData.append('player_position', formdata.playerposition);
		formData.append('department', formdata.department);
		formData.append('owner', false);
		formData.append('coowner', false);
		formData.append('captain', false);
		formData.append('marquee', false);
		formData.append('host', false);
		formData.append('player_value', 100);

		try {
			if (okRegister) {
				console.log('Sending request:', formData);
				const response = await axios.post(`api/signup`, formData, {
					headers: {
						'Content-Type': 'multipart/form-data'
					}
				});
				console.log('Response:', response.data);
				if (response.data) {
					isloading = false;
					isregistered = true;
				}
			} else {
				toast_message = `some error occured`;
				showerrortoast = true;
			}
		} catch (error) {
			isloading = false;
			showerrortoast = true;
			console.error('Error:', error.response);
			if (error.response && error.response.status === 400) {
				showerrortoast = true;
				toast_message = 'Error 400: Try again later or contact management';
				console.error('Bad Request Error:', error.response.data);
			} else {
				console.error('Error:', error.response);
			}
		}
	};

	function confirmmodal() {
		if (okRegister) {
			defaultModal = true;
		} else {
			showerrortoast = true;
		}
	}
</script>
{#if site_shutdown}
<div>
	Site is under maintainance for now it will be live later
</div>
{/if}
{#if !site_shutdown}
<div class="s flex min-h-screen items-center justify-center p-10">
	{#if showerrortoast}
		<div class="absolute right-0 top-0">
			<Toast color="orange">
				<svelte:fragment slot="icon">
					<ExclamationCircleSolid class="h-5 w-5" />
					<span class="sr-only">Warning icon</span>
				</svelte:fragment>
				{toast_message}
			</Toast>
		</div>
	{/if}
	<div class="md:flex md:flex-row md:items-center md:justify-center">
		<div class="m-10 max-w-3xl space-y-4 rounded-2xl">
			<Carousel {images} {forward} let:Indicators let:Controls bind:index>
				<Controls />
				<Indicators />
			</Carousel>
			<Thumbnails {images} {forward} bind:index />
		</div>

		<div>
			<form>
				<div class="mb-2 grid gap-6 md:grid-cols-2">
					<div>
						<Label for="first_name" class="mb-2">First name</Label>
						<Input
							type="text"
							id="first_name"
							placeholder="John"
							bind:value={formdata.firstname}
							required
						/>
					</div>
					<div>
						<Label for="last_name" class="mb-2">Last name</Label>
						<Input
							type="text"
							id="last_name"
							placeholder="Doe"
							bind:value={formdata.lastname}
							required
						/>
					</div>
				</div>

				<div class="mb-2">
					<Label for="email" class="mb-2">Email address</Label>
					<Input
						type="email"
						id="email"
						placeholder="john.doe@company.com"
						bind:value={formdata.email}
						required
					/>
				</div>

				<div class="mb-2">
					<Label for="username" class="mb-2">Username</Label>
					<Input
						type="text"
						id="username"
						placeholder="Username"
						bind:value={formdata.username}
						required
					/>
				</div>

				<div class="flex flex-row space-x-5">
					<div class="mb-2 w-1/2">
						<Label for="department">Department</Label>
						<Select id="department" items={department_list} bind:value={formdata.department} />
					</div>

					<div class="mb-2 w-1/2">
						<Label for="year">Year</Label>
						<Select id="year" items={year_list} bind:value={formdata.year} />
					</div>
				</div>
				<div class="mb-1 mt-1">
					<div class="mb-1 flex items-center justify-center">
						<Checkbox aria-describedby="helper-checkbox-text" bind:checked={isplayer}
							>are you a player ?</Checkbox
						>
					</div>

					{#if isplayer}
						<div>
							<div class="mb-2 mt-2">
								<Label for="year">Select your playing role</Label>
								<Select
									id="playing_position"
									items={role_list}
									bind:value={formdata.playerposition}
									placeholder="Select your playing role"
								/>
							</div>
						</div>
					{/if}
				</div>

				<Label for="with_helper" class="pb-2">Upload Image File</Label>
				<Fileupload id="with_helper" class="mb-2" on:change={handlefilechange} />
				<!-- <div class="flex flex-row"> -->
					<Helper>NOTE:</Helper>
					<Helper>MAX IMAGE FILE LIMIT : {fileSizeLimitMB}MB</Helper>
					<Helper>FILE NAME SHOULD BE WITHIN 20 CHARACTERS LIMIT</Helper>
				<!-- </div> -->
				<div class="flex justify-between">
					<Button on:click={confirmmodal} class="mt-2">Submit</Button>
				</div>
				<div class="mt-2 min-w-full">
					<GradientButton color="pinkToOrange" class="min-w-full">
						<a href="/list">Let's see the players!</a></GradientButton
					>
				</div>

				<Modal title="Are you sure you want to submit" bind:open={defaultModal} autoclose>
					<p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
						Are you sure about the data provided in the Sign up form ?
					</p>
					<svelte:fragment slot="footer">
						<Button on:click={handlesubmit}>Yes, Submit</Button>
						<Button color="alternative">Decline</Button>
					</svelte:fragment>
				</Modal>
				{#if isloading}
					<Modal title="Are you sure you want to submit" bind:open={isloading} autoclose={false}>
						<svelte:fragment slot="footer">
							<Button disabled>
								<Spinner class="me-3" size="4" color="white" />
								Loading ...
							</Button>
						</svelte:fragment>
					</Modal>
				{/if}
				{#if isregistered}
					<Modal title="you are successfully registered !" bind:open={isregistered} autoclose>
						<div>Click on Continue to Join your fellow players</div>
						<Button on:click={() => (window.location.href = '/list')}>Continue</Button>
					</Modal>
				{/if}
			</form>
		</div>
	</div>
</div>
<Footer class="mb-2 p-2">
	<hr class="my-6 border-gray-200 dark:border-gray-700 sm:mx-auto lg:my-8" />
	<FooterCopyright href="/" by="ICL-2024" />
</Footer>
{/if}
