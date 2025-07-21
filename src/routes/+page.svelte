<script lang="ts">
	import { onMount } from 'svelte';

	let clicks = 0;
	let timeout: ReturnType<typeof setTimeout>;
	function dummyclicks() {
		clicks++;
		
		clearTimeout(timeout);
		timeout = setTimeout(() => {
			clicks = 0;
		}, 2000);

		if (clicks >= 7) {
			clicks = 0;
			window.open("https://www.youtube.com/watch?v=xvFZjo5PgG0&list=RDxvFZjo5PgG0&start_radio=1", "_blank");
		}
	}

	let language = "";
	let description = "";

	async function getIdea() {
		const randomNumber = Math.floor(Math.random()*20) + 1;
		const request = await fetch("https://ai.hackclub.com/chat/completions", {
			method: "POST",
			headers: {
				"Content-Type": "application/json"
			},
			body: JSON.stringify({
				"messages": [{"role": "user", "content": `Generate a unique, creative, and uncommon project idea for a random programming language. To ensure it is randomized, use this list only: [\"JavaScript\", \"TypeScript\", \"Python\", \"Java\", \"C#\", \"C++\", \"C\", \"Go\", \"Swift\", \"Kotlin\", \"Ruby\", \"PHP\", \"Dart\", \"Rust\", \"Scala\", \"Objective-C\", \"Perl\", \"Lua\", \"R\", \"Groovy\"]. Pick the ${randomNumber} Item in the list. The project must be a practical, visual, or web-based project — NOT a purely command-line or backend-only project, and reccomended not a phone application, recommended sociel platfomrs (like discord, slack...) bots, websites, or games. DO NOT pick Haskell, Lisp, Elm, or other purely functional or academic languages. The project must take a teenager about 5+ hours and be realistic to build. Format the response ONLY as JSON in this structure: [ { \"language\": \"(the language)\", \"project\": \"(The project description)\" } ] and output absolutely nothing else, not additinal text, no nothing!.`}]
			})
		});
		if (!request.ok) {
			throw new Error(`Error ${request.status}`);
		}
		const response = await request.json();
		let contentInString = response.choices[0].message.content;
		contentInString = contentInString.replace(/```json\n?/g, '').replace(/```/g, '').trim();
		const content = JSON.parse(contentInString);
		language = content[0].language;
		description = content[0].project;
		const aiTextElem = document.getElementById("aitext");
		if (aiTextElem) {
			aiTextElem.innerHTML = `<strong>Language:</strong><br>${language}<br><br><br><strong>Description:</strong><br>${description}`;
		}
	}
	onMount(() => {
		getIdea();
	});
</script>

<div class="absolute inset-0 -z-10 h-full w-full bg-orange-100">
<div class="absolute left-0 right-0 top-0 -z-10 m-auto h-[310px] w-[310px] rounded-full bg-fuchsia-400 opacity-20 blur-[100px]"></div>
</div>
<div class="flex flex-col items-center p-4 sm:p-22 min-h-screen justify-between">
<div></div>
<div class="flex flex-col items-center px-4">
	<div class="relative flex items-center justify-center">
		<img src="/images/For Dummies logo.png" alt="Logo" class="absolute w-16 h-16 sm:w-20 sm:h-20 md:w-24 md:h-24 object-contain -translate-y-8 sm:-translate-y-12 md:-translate-y-15 -translate-x-35 sm:-translate-x-32 md:-translate-x-85 rotate-325 hover:scale-125 transition-transform duration-300"/>
		<h1 class="text-center">
			<button type="button" on:click={dummyclicks} class="text-4xl sm:text-6xl md:text-8xl text-amber-500 font-oi text-center" style="font-family: 'Oi'">DUMMIES</button>
		</h1>
	</div>
    <div class="h-1 bg-gradient-to-r from-transparent via-amber-500 to-transparent my-4 sm:my-8 mx-auto w-3/4"></div>
	<div class="flex flex-col sm:flex-row gap-4 items-center">
		<button class="shadow-lg/20 px-4 py-3 sm:px-6 sm:py-7 bg-amber-500 text-white rounded-lg font-extrabold hover:scale-105 transform transition-transform text-sm sm:text-base">SIGN UP</button>
		<button class="shadow-lg/20 px-4 py-3 sm:px-6 sm:py-7 bg-amber-500 text-white rounded-lg font-extrabold hover:scale-105 transform transition-transform text-sm sm:text-base">SUBMIT PROJECT</button>
	</div>
</div>
<a href="#WAWD">
<div class="pt-4 sm:pt-8">
	<p class="text-gray-600 text-sm sm:text-base text-center">Scroll down</p>
	<div class="text-gray-600 text-xl sm:text-2xl animate-bounce text-center">
		↓
	</div>
</div>
</a>

</div>
<div class="relative flex flex-col items-center p-4 sm:p-22 -mt-1">
<div class="absolute inset-0 -z-10 h-full w-full bg-purple-100">
<div class="absolute left-0 right-0 top-0 -z-10 m-auto h-[310px] w-[310px] rounded-full bg-fuchsia-400 opacity-20 blur-[100px]"></div>
</div>

<h2 id="WAWD" class="text-3xl sm:text-4xl md:text-6xl py-6 sm:py-12 text-amber-500 font-oi text-center px-4" style="font-family: 'Oi'">WHAT ARE WE DOING?</h2>
<div class="py-8 text-amber-700 text-lg sm:text-2xl md:text-4xl text-center px-2" style="font-family: 'Kirang Haerang'"><strong>*You*</strong> Learn A New programming language, and build a project with it<br><strong>*We*</strong> Give you a book of choice from <u><a href=https://www.dummies.com target="_blank">dummies.com</a></u></div>

</div>

<!--asking the ai, for a idea and a project-->
<div class="relative flex flex-col items-center p-4 sm:p-22 -mt-1">
<div class="absolute inset-0 -z-10 h-full w-full bg-gray-50">
<div class="absolute left-0 right-0 top-0 -z-10 m-auto h-[310px] w-[310px] rounded-full bg-fuchsia-400 opacity-20 blur-[100px]"></div>
</div>

<h2 class="text-3xl sm:text-4xl md:text-6xl py-6 sm:py-12 text-amber-500 font-oi text-center px-4" style="font-family: 'Oi'">PROJECT SUGGESTION</h2>
<div id="aitext" class="py-8 text-amber-700 text-lg sm:text-2xl md:text-4xl text-center px-2" style="font-family: 'Kirang Haerang'"><strong>Language:</strong><br>null<br><br><br><strong>Description:</strong><br>null</div>
<button class="shadow-lg/20 px-6 py-2 sm:px-8 sm:py-3 bg-amber-500 text-white rounded-lg font-extrabold hover:scale-105 transform transition-transform text-sm sm:text-base" on:click={getIdea}>RE-ROLL</button>

</div>
<!--faq-->
<div class="relative flex flex-col items-center p-4 sm:p-22">
<div class="absolute inset-0 -z-10 h-full w-full bg-amber-200">
<div class="absolute left-0 right-0 top-0 -z-10 m-auto h-[310px] w-[310px] rounded-full bg-fuchsia-400 opacity-20 blur-[100px]"></div>
</div>

<h2 class="text-3xl sm:text-4xl md:text-6xl py-6 sm:py-12 text-amber-500 font-oi text-center px-4" style="font-family: 'Oi'">FAQ</h2>
<div class="py-8 text-amber-700 text-lg sm:text-2xl md:text-4xl text-center px-2" style="font-family: 'Kirang Haerang'">
<strong><li>What counts as a new programming language?</li></strong><br>Any programming language that you never tried to learn, and never used for any projects<br><br><br>
<strong><li>Can I double dip with SOM?</li></strong><br>Answer Soon...<br><br><br>
<strong><li>What is the time requirement?</li></strong><br>At least 10 hours<br><br><br>
</div>
</div>
<div class="text-center text-gray-600 text-sm mt-8 pb-4">Made with &lt;3 by <a href="https://github.com/pkd2210" target="_blank" class="text-amber-600 hover:text-amber-700 underline">pkd2210</a></div>
