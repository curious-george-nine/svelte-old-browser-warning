<script lang="ts">
	import { fly } from 'svelte/transition';

	import OldBrowserDetector from 'old-browser-detector';

	import { onMount } from 'svelte';

	export let recommendedBrowser: 'Chrome' | 'Firefox' | 'Microsoft Edge' = 'Chrome';

	export let testing = false;

	let recommendedBrowserLink: string = 'no link still';

	let isBrowserOld: boolean;

	let countryCode: string;
	let languageCode: string;

	onMount(async () => {
		const getIpRes = await fetch('https://api.ipify.org/?format=json');
		const getIpData = await getIpRes.json();
		const ip = getIpData.ip;

		const getIpInfoRes = await fetch(`https://ipinfo.io/${ip}?token=d7c347df6032ca`);
		const getIpInfoData = await getIpInfoRes.json();

		countryCode = getIpInfoData.country;

		languageCode = navigator.language.split('-')[0];

		const countryAndLanguageCode: string = `${languageCode}-${countryCode}`;

		console.log(countryAndLanguageCode);

		switch (recommendedBrowser) {
			case 'Chrome':
				recommendedBrowserLink = `https://www.google.com/intl/${countryAndLanguageCode}/chrome/`;
				break;
			case 'Firefox':
				recommendedBrowserLink = `https://www.mozilla.org/${countryAndLanguageCode}/firefox/new/`;
				break;
			case 'Microsoft Edge':
				recommendedBrowserLink = `https://www.microsoft.com/${countryAndLanguageCode}/edge/download`;
				break;
		}

		const detector = new OldBrowserDetector(null, function () {});

		isBrowserOld = testing ? true : detector.detect();

		if (isBrowserOld) document.body.style.overflow = 'hidden';
	});

	function isBrowserOldSwitch() {
		setTimeout(() => {
			isBrowserOld = !isBrowserOld;

			document.body.style.overflow = '';
		}, 500);
	}
</script>

{#if isBrowserOld}
	<div
		class="h-screen w-screen bg-black bg-opacity-30 absolute top-0 right-0 flex justify-center items-center"
		in:fly={{ y: 30 }}
		out:fly={{ y: 30 }}
	>
		<div
			class="w-72 md:w-96 md:text-lg xl:w-[30rem] xl:py-12 2xl:w-[32rem] 2xl:px-12 2xl:text-xl bg-white rounded-md px-8 py-6 shadow-md font-bold space-y-6 md:duration-200"
		>
			<p>
				Looks like you're browser is old.
				<br />
				The website owner is recommending switching to
				<a
					href={recommendedBrowserLink}
					target="_blank"
					rel="noreferrer noopener"
					class="text-violet-400 underline hover:text-violet-400/60 transition"
					>{recommendedBrowser}</a
				>.
			</p>
			<div>
				<button
					class="bg-red-300 py-2 px-5 rounded-md font-normal shadow-lg hover:bg-red-300/80 active:shadow-sm duration-500 xl:px-7 xl:py-3"
					on:click={() => isBrowserOldSwitch()}
				>
					Ignore and close
				</button>
			</div>
		</div>
	</div>
{/if}

<style lang="postcss">
	@tailwind base;
	@tailwind components;
	@tailwind utilities;

	* {
		font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu,
			Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
	}
</style>
