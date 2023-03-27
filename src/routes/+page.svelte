<!-- TODO: make this a PWA as well, that would be cool -->
<script lang="ts">
	import { each, onMount, space, onDestroy } from "svelte/internal";
	import { fly, slide, fade, blur, crossfade } from "svelte/transition";

	//Make python sleep because idk how setTimeout works
	const sleep = (milliseconds: number) => {
		return new Promise((resolve) => setTimeout(resolve, milliseconds));
	};

	//Initialize all Categories and Hover Elements
	let allCategories: string[] = [
		"A Boy's Name",
		"U.S. Cities",
		"Things That Are Cold",
		"School Supplies",
		"Pro Sports Teams",
		"Insects",
		"Breakfast Foods",
		"Furniture",
		"T.V. Shows",
		"Things That Are Found in the Ocean",
		"Presidents",
		"Product Names",
		"Appliances",
		"Types of Drink",
		"Personality Traits",
		"Articles of Clothing",
		"Desserts",
		"Car Parts",
		"Things Found on a Map",
		"Athletes",
		"4-Letter Words",
		"Items in a Refrigerator",
		"Farm Animals",
		"Street Names",
		"Things on a Beach",
		"Colors",
		"Tools",
		"A Girl's Name",
		"Villains/Monsters",
		"Footwear",
		"Something You're Afraid Of",
		"Terms of Measurement",
		"Book Titles",
		"Heroes",
		"Gifts/Presents",
		"Kinds of Dances",
		"Things That Are Black",
		"Vehicles",
		"Tropical Locations",
		"College Majors",
		"Dairy Products",
		"Things in a Souvenir Shop",
		"Items in Your Purse/Wallet",
		"Famous Females",
		"Medicine/Drugs",
		"Things Made of Metal",
		"Hobbies",
		"People in Uniform",
		"Things You Plug In",
		"Animals",
		"Languages",
		"Names Used in the Bible",
		"Junk Food",
		"Things That Grow",
		"Companies",
		"Video Games",
		"Electronic Gadgets",
		"Board Games",
		"Things That Use a Remote",
		"Card Games",
		"Internet Lingo",
		"Offensive Words",
		"Wireless Things",
		"Computer Parts",
		"Software",
		"Websites",
		"Game Terms",
		"Things in a Grocery Store",
		"Reasons to Quit Your Job",
		"Things That Have Stripes",
		"Tourist Attractions",
		"Diet Foods",
		"Things Found in a Hospital",
		"Food/Drink That Is Green",
		"Weekend Activities",
		"Acronyms",
		"Seafood",
		"Christmas Songs",
		"Words Ending in -N",
		"Words With Double Letters",
		`Children's Books`,
		"Things Found at a Bar",
		"Sports Played Indoors",
		"Names Used in Songs",
		"Foods You Eat Raw",
		"Places in Europe",
		"Olympic Events",
		"Things You See at the Zoo",
		"Math Terms",
		"Animals in Books or Movies",
		"Things to Do at a Party",
		"Sandwiches",
		"Items in a Catalog",
		"World Leaders/Politicians",
		"School Subjects",
		"Excuses for Being Late",
		"Ice Cream Flavors",
		"Things That Jump/Bounce",
		"Television Stars",
		"Things in a Park",
		"Foreign Cities",
		"Stones/Gems",
		"Musical Instruments",
		"Nicknames",
		"Things in the Sky",
		"Pizza Toppings",
		"Colleges/Universities",
		"Fish",
		"Countries",
		"Things That Have Spots",
		"Historical Figures",
		"Terms of Endearment",
		"Items in This Room",
		"Fictional Characters",
		"Menu Items",
		"Magazines",
		"Capitals",
		"Kinds of Candy",
		"Items You Save Up to Buy",
		"Something You Keep Hidden",
		"Items in a Suitcase",
		"Things With Tails",
		"Sports Equipment",
		"Crimes",
		"Things That Are Sticky",
		"Awards/Ceremonies",
		"Cars",
		"Spices/Herbs",
		"Bad Habits",
		"Cosmetics/Toiletries",
		"Celebrities",
		"Cooking Utensils",
		"Reptiles/Amphibians",
		"Parks",
		"Leisure Activities",
		"Things You're Allergic To",
		"Restaurants",
		"Notorious People",
		"Fruits",
		"Things in a Medicine Cabinet",
		"Toys",
		"Household Chores",
		"Bodies of Water",
		"Authors",
		"Halloween Costumes",
		"Weapons",
		"Things That Are Round",
		"Words Associated With Exercise",
		"Sports",
		"Song Titles",
		"Parts of the Body",
		"Ethnic Foods",
		"Things You Shout",
		"Birds",
		"Methods of Transportation",
		"Items in a Kitchen",
		"Flowers",
		"Things You Replace",
		"Famous Duos and Trios",
		"Things Found in a Desk",
		"Vacation Spots",
		"Diseases",
		"Words Associated With Money",
		"Items in a Vending Machine",
		"Movie Titles",
		"Games",
		"Things That You Wear",
		"Beers",
		"Things at a Circus",
		"Vegetables",
		"States",
		"Things You Throw Away",
		"Occupations",
		"Cartoon Characters",
		"Types of Drinks",
		"Musical Groups",
		"Store Names",
		"Things at a Football Game",
		"Trees",
		"Kinds of Soup",
		"Things Found in New York",
		"Things You Get Tickets For",
		"Things You Do at Work",
		"Foreign Words Used in English",
		"Things You Shouldn't Touch",
		"Spicy Foods",
		"Things at a Carnival",
		"Things You Make",
		"Places to Hang Out",
		"Honeymoon Spots",
		"Things You Buy for Kids",
		"Reasons to Take Out a Loan",
		"Things to Do on a Date",
		"Historic Events",
		"Things You Do Every Day",
		"Things You Get in the Mail",
		"Things You Sit On",
		"Reasons to Make a Phone Call",
		"Titles People Can Have",
		"Things That Have Buttons",
		"Things That Have Wheels",
		"Reasons to Call 911",
		"Ways to Kill Time",
		"Things That Can Get You Fired",
	];
	let pauseHovered = false;
	let refreshHovered = false;

	// CHOOSE OR REFRESH LETTER
	let letter: string = randomLetter();
	function randomLetter() {
		//Only scattergories letters
		const characters = "abcdefghijklmnoprstw";
		return characters.charAt(Math.floor(Math.random() * characters.length)).toUpperCase();
	}

	//   CHOOSE CATEGORIES
	//  empty and choose 12 new categories each time refresh is run.
	//  Remove selected categories from the big list so that they're not reused.
	let possibleCategories: string[] = allCategories;
	let categories: string[] = refreshCategories();
	function refreshCategories() {
		//Refresh the letter as well on list refresh
		letter = randomLetter();
		let selectedCategories = [];
		for (let i = 0; i < 12; i++) {
			const randomIndex = Math.floor(Math.random() * possibleCategories.length);
			const selectedCategory = possibleCategories[randomIndex];
			possibleCategories.splice(randomIndex, 1);
			selectedCategories.push(selectedCategory);
		}
		return selectedCategories;
	}

	//SET AND COUNT DOWN TIMER...
	//Should probably make this pause on leaving the page but it should be fine
	//Number always resets back to time entered by user (resetTo)
	//StartTimer and pause timer do exactly that and are called by the later "$:" statement

	let resetTo: number = 120; //Bound to value of change time
	let time = resetTo;
	let interval: any;
	let alertTime = false;
	function startTimer() {
		paused = false;
		interval = setInterval(function () {
			//If out of time, run animate saying "Time's up."
			//AlertTime is used because text needs to be replaced before the second animateRefresh call for actual refreshing
			if (time === 0) {
				clearInterval(interval);
				alertTime = true;
				animateRefresh(true);
			} else {
				time--;
			}
		}, 1000);
	}

	function stopTimer() {
		clearInterval(interval);
		paused = true;
	}

	//Used for changing the time. ChangeTimeSelected to handle clicks that aren't confirm.
	let changeTimeSelected = false;
	function confirmTimeClicked() {
		if (resetTo > 1000 || resetTo <= 0) {
			resetTo = 120;
		}
		time = resetTo;
		changeTimeSelected = false;
	}

	// Set text and Icon for pause/play box depending on the paused state
	let playPause: string, pausedIcon: string;
	let paused = true;
	$: if (!paused) {
		startTimer();
		playPause = "Pause";
		pausedIcon = "/pause.png";
	} else {
		stopTimer();
		playPause = "Play";
		pausedIcon = "/play.png";
	}

	// Animate Refresh. Either for resetting or when the time runs out.
	let showReset = false; //Covers the background completely
	let spinRefresh = false; //Animate the spinny image and accompanying text
	async function animateRefresh(timesUp: boolean = false) {
		if (timesUp) {
			showReset = true; //Swipe in
			await sleep(2000); //Let layer stay longer when time is up
			showReset = false; // Swipe out
			alertTime = false;
			return;
		}
		//Spin and wait a sec, then swipe in and out with a one second delay
		spinRefresh = true;
		await sleep(200);
		showReset = true;
		await sleep(1000);
		showReset = false;
		spinRefresh = false;
	}
</script>

<!-- This shit is all over the place and I hope to make improvements but it looks and works really good :D -->
<!-- Really shouldn't be in one file, but really why not? -->
<!-- TODO: Make this responsive. -->


<!-- Whole thing is just a big ole grid. LARGE SCREEN SIZE-->
<div class="h-[97vh] scale-95 font-serif text-[#222222] lg:flex hidden">
	<!-- FIRST COLUMN -->
	<div class="flex flex-col h-full xl:w-[30%] w-[40%]">
		<!-- Top Half, just the letter. Reroll function is pretty simple -->
		<div class="relative h-1/2 border-4 border-neutral-900 border-b-0 stripes flex justify-center items-center">
			<div class="top-0 absolute flex justify-between w-full">
				<p class="text-2xl left-0 mx-3 my-1">Letter</p>
				<button
					class="text-2xl text-neutral-900/70 hover:text-neutral-900/90 {!paused ? 'opacity-0' : 'opacity-100'} transition-all right-0 mx-3 my-1"
					on:click={() => {
						letter = randomLetter();
					}}>Re-roll</button
				>
			</div>
			<h1 class="font-semibold font-sans text-[18rem]">{letter}</h1>
		</div>

		<!-- Bottom Half, time and pause/play. A lot happening here so read more in depth -->
		<div class="flex h-1/2">
			<!-- Time. Shows time and allows for changing time. When time is changed, it is saved to be used in future rounds. -->
			<div class="w-1/2 border-4 border-neutral-900 border-r-0 relative flex justify-center items-center">
				<div class="top-0 absolute flex justify-between w-full">
					<p class="text-2xl left-0 mx-3 my-1">Time</p>
					<button
						class="text-2xl left-0 mx-3 my-1 text-neutral-900/70 hover:text-neutral-900/90 transition-all"
						on:click={() => {
							paused = true;
							if (changeTimeSelected) {
								confirmTimeClicked();
							} else {
								changeTimeSelected = true;
							}
						}}>Change</button
					>
				</div>

				<!-- When change is selected, open an editor of sorts. Clicking confirm, change again, or pressing play are all valid escapes -->
				{#if changeTimeSelected}
					<form class="flex flex-col items-center justify-center">
						<input class="border-2 border-neutral-800 border-b-0 text-3xl w-44 h-10" type="number" placeholder={`${time - 1}`} bind:value={resetTo} />
						<button class="w-full bg-neutral-800 text-white hover:bg-neutral-900 h-8" on:click={confirmTimeClicked}>Confirm</button>
					</form>
				{:else}
					<h1 class="text-8xl">{time}</h1>
				{/if}
			</div>

			<!-- PLAY / PAUSE -->
			<div
				class="w-1/2 border-4 border-neutral-900 relative flex justify-center items-center {pauseHovered ? 'dots' : ''}"
				on:mouseenter={() => {
					pauseHovered = true;
				}}
				on:mouseleave={() => {
					pauseHovered = false;
				}}
			>
				<!-- Key makes it so that each time paused changes, the animations are reset. Most of this stuff is handled in the script portion though-->
				{#key paused}
					<div class="absolute top-0 flex w-full">
						<p class="text-2xl my-1 mx-3" transition:fly={{ delay: 100, duration: 400, y: 20 }}>{playPause}</p>
					</div>
					<img
						src={pausedIcon}
						alt="{playPause} Game"
						class="{paused ? 'w-56' : 'w-40'} cursor-pointer absolute"
						in:fly={{ delay: 100, x: -20 }}
						out:fly={{ duration: 100, x: 20 }}
						on:click={() => {
							if (changeTimeSelected) {
								confirmTimeClicked();
							}
							paused = !paused;
						}}
						on:keypress={() => {
							if (changeTimeSelected) {
								confirmTimeClicked();
							}
							paused = !paused;
						}}
					/>
				{/key}
			</div>
		</div>
	</div>
	<!-------------------------------------------------- End first column -->

	<!-- SECOND COLUMN -->
	<div class="flex flex-col h-full w-[45%] xl:w-[55%] font-sans">
		<!-- Categories Heading -->
		<!-- At some point, if it makes sense, have option to add category or increase number of categories -->
		<div class="w-full border-4 border-l-0 border-neutral-900 h-[6%] flex items-center py-5 justify-between">
			<h1 class="text-2xl m-2 font-semibold">Categories</h1>
		</div>

		<!-- List of Categories -->
		<!-- Categories can run and will be replaced with "Out of categories" -->
		<ol class="w-full flex flex-col p-2 border-4 border-neutral-900 border-t-0 h-full border-l-0 " transition:slide>
			{#each categories as category, index}
				<li class="text-2xl py-2 border-b font-semibold relative w-fit" out:fade>
					{++index} <span class="2xl:text-4xl text-3xl  mx-4">{category ? category : "Out of categories"}</span>
					{#if paused}
						<div class="absolute inset-0 bg-[#222222]" in:slide={{ delay: index * 40 }} out:slide />
					{/if}
				</li>
			{/each}
		</ol>
	</div>
	<!--------------------- End Second Column -->

	<!-- THIRD COLUMN, RESET -->
	<!-- Pretty straight forward here. -->
	<div
		class="h-full border-4 border-neutral-900 border-l-0 w-[15%] flex justify-center items-center cursor-pointer {refreshHovered ? 'dots' : ''}"
		on:mouseenter={() => {
			refreshHovered = true;
		}}
		on:mouseleave={() => {
			refreshHovered = false;
		}}
		on:click={() => {
			animateRefresh();
			// Break in between to allow screen to be covered before changing UI
			setTimeout(() => {
				categories = refreshCategories();
				time = resetTo;
				paused = true;
			}, 600);
		}}
		on:keypress={() => {
			animateRefresh();
			setTimeout(() => {
				categories = refreshCategories();
				time = resetTo;
				paused = true;
			}, 600);
		}}
	>
		<!-- Refresh Spins and Words Drop down. -->
		<div class="flex flex-col justify-center items-center">
			<img
				src="refresh.png"
				alt="Refresh List"
				class="w-20 transition-all duration-300 {time === 0 ? 'animate-bounce' : ''} {spinRefresh ? 'rotate-180 opacity-0' : 'rotate-0 opacity-100'}"
			/>
			<h4 class="text-3xl my-5 {spinRefresh ? 'translate-y-4 opacity-0' : 'translate-y-0 opacity-100'} transition-all duration-300">New List</h4>
		</div>
	</div>
	<!-------- End Third Column ------------->



	<!-- TIMES UP and RESETTING overlay. Swipe in and out! -->
	{#if showReset}
		<div class="absolute inset-0 bg-[#222222] w-full opactiy-100 flex items-center justify-center" transition:slide={{ duration: 400, axis: "x" }}>
			<h1 class="text-9xl text-white font-bold">{alertTime ? "Time's Up" : "Restarting..."}</h1>
		</div>
	{/if}
	<!----------End Swipe Overlay----------->
</div>








<!-- MD SCREEN SIZES -->
<div class="flex h-screen text-[#222222] lg:hidden font-sans">
	<!-- FIRST COLUMN : Letter, time, and play/pause -->
	<div class="flex flex-col w-1/3 h-full border-l-4 border-r-4 border-neutral-800 font-serif">

		<!-- LETTER -->
		<div class="h-1/3 w-full border-b-4 border-neutral-800 relative flex items-center justify-center stripes">
			<div class="top-0 absolute flex justify-between w-full">
				<p class="md:text-2xl sm:text-lg left-0 sm:mx-3 mx-1 my-1">Letter</p>
				<button
					class="md:text-2xl sm:text-lg text-neutral-900/70 hover:text-neutral-900/90 {!paused
						? 'opacity-0'
						: 'opacity-100'} transition-all right-0 sm:mx-3 mx-1 my-1"
					on:click={() => {
						letter = randomLetter();
					}}>Re-roll</button
				>
			</div>
			<h1 class="font-semibold font-sans md:text-[12rem] text-[7rem]">{letter}</h1>
		</div>

		<!-- Time -->
		<div class="h-1/3 w-full border-b-4 border-neutral-800 relative flex items-center justify-center">
			<div class="top-0 absolute flex justify-between w-full">
				<p class="md:text-2xl sm:text-lg sm:mx-3 mx-1 my-1">Time</p>
				<button
					class="md:text-2xl sm:text-lg sm:mx-3 mx-1 my-1 text-neutral-900/70 hover:text-neutral-900/90 transition-all"
					on:click={() => {
						paused = true;
						if (changeTimeSelected) {
							confirmTimeClicked();
						} else {
							changeTimeSelected = true;
						}
					}}>Change</button
				>
			</div>
			{#if changeTimeSelected}
				<form class="flex flex-col items-center justify-center">
					<input
						class="border-2 border-neutral-800 border-b-0 text-3xl md:w-44 sm:w-36 w-24 h-10"
						type="number"
						placeholder={`${time - 1}`}
						bind:value={resetTo}
					/>
					<button class="w-full bg-neutral-800 text-white hover:bg-neutral-900 h-8" on:click={confirmTimeClicked}>Confirm</button>
				</form>
			{:else}
				<h1 class="sm:text-7xl text-6xl md:text-8xl">{time}</h1>
			{/if}
		</div>

		<!-- Play/Pause -->
		<div class="h-1/3 w-full relative flex items-center justify-center">
			<!-- Key makes it so that each time paused changes, the animations are reset. Most of this stuff is handled in the script portion though-->
			{#key paused}
				<div class="absolute top-0 flex w-full">
					<p class="text-2xl my-1 mx-3" transition:fly={{ delay: 100, duration: 400, y: 20 }}>{playPause}</p>
				</div>
				<img
					src={pausedIcon}
					alt="{playPause} Game"
					class="{paused ? 'md:w-56 w-40' : 'md:w-40 w-32'} cursor-pointer absolute"
					in:fly={{ delay: 100, x: -20 }}
					out:fly={{ duration: 100, x: 20 }}
					on:click={() => {
						if (changeTimeSelected) {
							confirmTimeClicked();
						}
						paused = !paused;
					}}
					on:keypress={() => {
						if (changeTimeSelected) {
							confirmTimeClicked();
						}
						paused = !paused;
					}}
				/>
			{/key}
		</div>
	</div>


	<!-- SECOND COLUMN : categories and refresh -->
	<div class="flex flex-col w-2/3 h-full">

			<!-- Categories Heading -->
			<div class=" h-4/5 w-full  flex items-center justify-between flex-col">
				<div class="w-full border-b-4 border-r-4 border-neutral-900">
				<h1 class="text-xl m-2 font-semibold">Categories</h1>
			</div>
			

			<!-- List of Categories -->
			<!-- Categories can run and will be replaced with "Out of categories" -->
			<ol class="w-full flex flex-col p-2 border-r-4 border-neutral-900 border-t-0 h-full border-l-0 " transition:slide>
				{#each categories as category, index}
					<li class="text-sm sm:text-md md:text-lg sm:py-2 py-1 border-b font-semibold relative w-fit" out:fade>
						{++index} <span class="text-[1rem] sm:text-xl md:text-2xl mx-4">{category ? category : "Out of categories"}</span>
						{#if paused}
							<div class="absolute inset-0 bg-[#222222]" in:slide={{ delay: index * 40 }} out:slide />
						{/if}
					</li>
				{/each}
			</ol>
		</div>


		<!-- REFRESH -->
			<div
				class=" h-1/5 border-4 border-neutral-900 border-l-0 border-b-0  flex justify-center items-center cursor-pointer dots"
				on:mouseenter={() => {
					refreshHovered = true;
				}}
				on:mouseleave={() => {
					refreshHovered = false;
				}}
				on:click={() => {
					animateRefresh();
					// Break in between to allow screen to be covered before changing UI
					setTimeout(() => {
						categories = refreshCategories();
						time = resetTo;
						paused = true;
					}, 600);
				}}
				on:keypress={() => {
					animateRefresh();
					setTimeout(() => {
						categories = refreshCategories();
						time = resetTo;
						paused = true;
					}, 600);
				}}
			>
				<!-- Refresh Spins and Words Drop down. -->
				<div class="flex flex-col justify-center items-center">
					<img
						src="refresh.png"
						alt="Refresh List"
						class="sm:w-20 w-16 transition-all duration-300 {time === 0 ? 'animate-bounce' : ''} {spinRefresh
							? 'rotate-180 opacity-0'
							: 'rotate-0 opacity-100'}"
					/>
					<h4 class="text-3xl sm:my-5 my-2 {spinRefresh ? 'translate-y-4 opacity-0' : 'translate-y-0 opacity-100'} transition-all duration-300">New List</h4>
				</div>
			</div>
		</div>
		{#if showReset}
		<div class="absolute inset-0 bg-[#222222] w-full opactiy-100 flex items-center justify-center" transition:slide={{ duration: 400, axis: "x" }}>
			<h1 class="md:text-8xl sm:text-6xl text-5xl text-white font-bold">{alertTime ? "Time's Up" : "Restarting..."}</h1>
		</div>
	{/if}
</div>
<!--------------END MD SCREEN SIZES -------------->



<!-- Shout out friend :) and me. -->
<p class="lg:text-xl text-sm absolute bottom-4 lg:mx-12 mx-4 hidden lg:block">
	Swellgarfo's <span class="underline underline-offset-2 hover:underline-offset-4 transition-all"
		><a href="https://swellgarfo.com/scattergories/">Scattergories List Generator</a></span
	> Remade by <span class="underline underline-offset-2 hover:underline-offset-4 transition-all"
	><a href="https://joemmalatesta.com/">Joe Malatesta</a></span
>
</p>

<!-- Some shit I snatched from the internet :demon: -->
<style>
	.stripes {
		background-image: repeating-linear-gradient(45deg, transparent, #2b2b2b33 12px, transparent 0px, transparent 0px);
	}

	.dots {
		background-image: radial-gradient(#22222281 1px, transparent 1px);
		background-size: 10px 10px;
		background-repeat: repeat;
	}
</style>
