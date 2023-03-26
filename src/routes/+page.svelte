<script lang="ts">
	import { each, onMount, space, onDestroy } from "svelte/internal";
	import { fly, slide, fade, blur, crossfade } from "svelte/transition";
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

	//   CHOOSE CATEGORIES
	let possibleCategories: string[] = allCategories;
	let categories: string[] = refreshCategories();
	function refreshCategories() {
		let selectedCategories = [];
		for (let i = 0; i < 12; i++) {
			const randomIndex = Math.floor(Math.random() * possibleCategories.length);
			const selectedCategory = possibleCategories[randomIndex];
			possibleCategories.splice(randomIndex, 1);
			selectedCategories.push(selectedCategory);
		}
		return selectedCategories;
	}




	// CHOSE OR REFRESH LETTER
	let letter: string = randomLetter();
	function randomLetter() {
		const characters = "abcdefghijklmnopqrstuvwxyz";
		return characters.charAt(Math.floor(Math.random() * characters.length)).toUpperCase();
	}




    //SET AND COUNT DOWN TIMER... MAKE IT STOP ON DESTROY BUT IDK HOW.
	let resetTo: number = 120;
	let time = resetTo;
	let countDown: number;
	countDown = setInterval(() => {
		if (!paused) {
			time--;
		}
		if (time === -1) {
			alert("Time's Up");
			time = 0;
			paused = true;
			clearInterval(countDown);
		}
	}, 1000);




	// Set paused text and Icon
	let playPause: string, pausedIcon: string;
	let paused = true;
	$: if (!paused) {
		playPause = "Pause";
		pausedIcon = "/pause.webp";
	} else {
		playPause = "Play";
		pausedIcon = "/play.png";
	}
</script>

<div class="flex h-screen scale-95 font-serif text-[#222222]">
	<!-- FIRST COLUMN -->
	<div class="flex flex-col h-full w-[30%]">
		<!-- Top Half -->
		<div class="relative h-1/2 border-4 border-neutral-900 border-b-0 stripes flex justify-center items-center">
			<div class="top-0 absolute flex justify-between w-full">
				<p class="text-3xl left-0 mx-3 my-1">Letter</p>
				<button
					class="text-3xl text-neutral-900/70 hover:text-neutral-900/90 transition-all right-0 mx-3 my-1"
					on:click={() => {
						letter = randomLetter();
					}}>Refresh</button
				>
			</div>
			<h1 class="font-semibold font-sans" style="font-size: 18rem ;">{letter}</h1>
		</div>
		<!-- Bottom Half -->
		<div class="flex h-1/2">
			<!-- Time -->
			<div class="w-1/2 border-4 border-neutral-900 border-r-0 relative flex justify-center items-center">
				<div class="top-0 absolute flex justify-between w-full">
					<p class="text-2xl left-0 mx-3 my-1">Time</p>
					<button class="text-2xl left-0 mx-3 my-1 text-neutral-900/70 hover:text-neutral-900/90 transition-all">Change</button>
				</div>

				<h1 class="text-8xl">{time}</h1>
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
				{#key paused}
					<div class="absolute top-0 flex w-full">
						<p class="text-2xl my-1 mx-3" transition:fly={{ delay: 100, duration: 400, y: 20 }}>{playPause}</p>
					</div>
					<img
						src={pausedIcon}
						alt="{playPause} Game"
						class="{paused ? 'w-56' : 'w-40'} cursor-pointer absolute"
						in:fly={{ delay: 100, x:-20 }}
						out:fly={{ duration: 100, x:20 }}
						on:click={() => {
							paused = !paused;
						}}
						on:keypress={() => {
							paused = !paused;
						}}
					/>
				{/key}
			</div>
		</div>
	</div>

	<!-- SECOND COLUMN -->
	<div class="flex flex-col h-full w-[55%] font-sans">
		<!-- Categories Heading -->
		<div class="w-full border-4 border-l-0 border-neutral-900 h-[6%] flex items-center py-5 justify-between">
			<h1 class="text-2xl m-2 font-semibold">Categories</h1>
		</div>

		<!-- List of Categories -->
		<ol class="w-full flex flex-col p-2 border-4 border-neutral-900 border-t-0 h-full border-l-0 " transition:slide>
			{#each categories as category, index}
				<li class="text-3xl py-[6px] border-b font-semibold relative w-fit" out:fade>
					{++index} <span class="text-5xl mx-4">{category ? category : "Ran of Categories"}</span>
					{#if paused}
						<div class="absolute inset-0 bg-[#222222]" in:slide={{ delay: index * 40 }} out:slide />
					{/if}
				</li>
			{/each}
		</ol>
	</div>

	<!-- THIRD COLUMN, RESET -->
	<div
		class="h-full border-4 border-neutral-900 border-l-0 w-[15%] flex justify-center items-center cursor-pointer {refreshHovered ? 'dots' : ''}"
		on:mouseenter={() => {
			refreshHovered = true;
		}}
		on:mouseleave={() => {
			refreshHovered = false;
		}}
		on:click={() => {
			categories = refreshCategories();
			time = resetTo;
		}}
		on:keypress={() => {
			categories = refreshCategories();
			time = resetTo;
		}}
	>
		<div class="flex flex-col justify-center items-center">
			<img src="refresh.png" alt="Refresh List" class="w-20" />
			<h4 class="text-3xl my-5">New List</h4>
		</div>
	</div>
</div>
<p class="text-xl absolute bottom-0 mx-12">Remake of Swellgarfo's <span class="underline"><a href="https://swellgarfo.com/scattergories/">Scattergories List Generator</a></span></p>

<style>
	.stripes {
		background-image: repeating-linear-gradient(45deg, transparent, #2b2b2b33 15px, transparent 0px, transparent 0px);
	}

	.dots {
		background-image: radial-gradient(#22222281 1px, transparent 1px);
		background-size: 10px 10px;
		background-repeat: repeat;
	}
</style>
