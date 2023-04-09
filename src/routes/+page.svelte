<script lang="ts">
	import { writable } from 'svelte/store';
	import InfoBox from '$lib/layout/InfoBox.svelte';
	import PageLayout from '$lib/layout/PageLayout.svelte';
	import Logo from '$lib/layout/Logo.svelte';
	import discord from './discord.svg';
	import github from './github.svg';
	import linkedIn from './linked-in.png';
	import skyline from './skyline.svg';
	import twitter from './twitter.svg';
	import Sponsors from './sponsoring/Sponsors.svelte';
	import WhatToExpect from './WhatYouCanExpect.svelte';
	import {
		isRegistrationOpen,
		registrationOpensAt,
		timeLeft
	} from '$lib/participants/registration';
	import { base } from '$app/paths';

	const countdown = writable<string>('');
	const canRegister = writable<boolean>(isRegistrationOpen());
	const updateCountdown = () => {
		const now = +new Date();
		if (registrationOpensAt <= now) {
			$canRegister = true;
			return;
		}
		const { days, hours, minutes, seconds } = timeLeft(now, registrationOpensAt);
		const timeAsStringArray = [
			days > 0 ? `${days} days` : '',
			hours > 0 ? `${hours} hours` : '',
			minutes > 0 ? `${minutes} minutes` : '',
			seconds > 0 ? `${seconds} seconds` : ''
		].filter((s) => s !== '');

		$countdown =
			timeAsStringArray.length > 1
				? `That's in <strong>${timeAsStringArray.slice(0, -1).join(', ')} and ${
						timeAsStringArray[timeAsStringArray.length - 1]
				  }</strong>.`
				: timeAsStringArray.length === 1
				? `Only <strong>${timeAsStringArray[0]}</strong> left!`
				: '';

		setTimeout(updateCountdown, 1000);
	};
	updateCountdown();
</script>

<PageLayout>
	<h1>Welcome to JSCraftCamp 2023! 🎉</h1>

	<section>
		<InfoBox title="When?">
			<p>June 30th & July 1st, 2023.</p>
			{#if !$canRegister}
				<p>Registration opens on May 1st, 2023. {@html $countdown}</p>
			{:else}
				<p><a href="{base}/registration">Registration is open!</a></p>
			{/if}
		</InfoBox>
		<InfoBox title="Where?">
			codecentric offices Munich, <a
				href="https://www.ecosia.org/search?method=index&q=August-Everding-Str+20+81671+M%C3%BCnchen"
				rel="external">August-Everding-Str 20, 81671 München</a
			>
		</InfoBox>
		<Logo style="max-width: 100%;" />
		<div>
			<a href="https://twitter.com/jscraftcamp" rel="external"
				><img src={twitter} alt="Twitter" /></a
			>
			<a href="https://github.com/jscraftcamp/website" rel="external"
				><img src={github} alt="GitHub" /></a
			>
			<a
				href="https://www.linkedin.com/events/jscraftcamp2023-jscc237033512998556028929/comments/"
				rel="external"><img src={linkedIn} alt="LinkedIn" height="128" width="128" /></a
			>
			<a href="https://discord.gg/3mHhaXVEjP" rel="external"><img src={discord} alt="Discord" /></a>
		</div>
		<img src={skyline} alt="Munich skyline" />
		<Sponsors />
		<WhatToExpect />
	</section>
</PageLayout>

<style>
	h1 {
		text-align: center;
	}
	section {
		display: flex;
		flex-flow: column;
		align-items: center;
		gap: 2em;
	}
	div {
		display: flex;
		flex-flow: row wrap;
		align-items: center;
		justify-content: center;
		gap: 1em;
	}
	div img {
		height: 40px;
		width: 40px;
	}
	img {
		max-width: 100%;
	}
</style>