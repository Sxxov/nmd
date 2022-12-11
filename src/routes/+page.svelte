<script lang="ts">
	import { goto } from '$app/navigation';
	import {
		ic_account_circle,
		ic_bluetooth,
		ic_delete,
		ic_edit,
		ic_emoji_symbols,
		ic_media_bluetooth_on,
		ic_pending,
		ic_search,
		ic_settings,
		ic_settings_remote,
	} from 'maic/two_tone';
	import Account from '../components/composed/Account.svelte';
	import Mod from '../components/composed/Mod.svelte';
	import type { IMod } from '../components/composed/mod/IMod';
	import ModEdit from '../components/composed/ModEdit.svelte';
	import {
		buttonsW,
		toppingsW,
	} from '../components/composed/nav/TopNav.svelte';
	import Settings from '../components/composed/Settings.svelte';
	import Variables from '../components/composed/Variables.svelte';
	import { dropIn } from '../core/transitioner/Transitioner';

	$toppingsW = [
		{
			icon: ic_account_circle,
			text: 'you',
			action() {
				isMenuActives[0] = true;
			},
		},
		{
			icon: ic_emoji_symbols,
			text: 'variables',
			action() {
				isMenuActives[1] = true;
			},
		},
		{
			icon: ic_settings,
			text: 'settings',
			action() {
				isMenuActives[2] = true;
			},
		},
	];

	$buttonsW = [
		{
			icon: ic_search,
			action() {
				console.log('search');
			},
		},
		{
			icon: ic_edit,
			action() {
				console.log('edit');
			},
		},
	];

	const isMenuActives = [false, false, false];

	let editI = 0;
	let isSavePromptActive = false;
	let isDeletePromptActive = false;
	let isModEditActive = false;

	let items: IMod[] = new Array(3).fill(undefined).map((_, i) => ({
		name: 'open sesame',
		whens: [
			{
				icon: ic_bluetooth,
				name: 'connected — "bluetooth_device"',
				action() {},
			},
		],
		dos: [
			{
				icon: ic_settings_remote,
				name: 'activate switch — "door"',
				action() {},
			},
			{
				icon: ic_media_bluetooth_on,
				name: 'play — "magic.mp3" on "Apple HomePod"',
				action() {},
			},
			{
				icon: ic_pending,
				name: 'wait — 5s',
				action() {},
			},
		],
		buttons: [
			{
				icon: ic_delete,
				action() {
					editI = i;
					isDeletePromptActive = true;
				},
			},
			{
				icon: ic_edit,
				action() {
					editI = i;
					setTimeout(() => {
						isModEditActive = true;
					});
				},
			},
		],
	}));
</script>

<div type="/" class="component" class:lock={isModEditActive}>
	<div class="modules">
		{#each items as item, i}
			<div in:dropIn={{ delay: i * 100 }}>
				<Mod {item} isExpanded={i === 0} />
			</div>
		{/each}
	</div>
	<ModEdit
		bind:mod={items[editI]}
		bind:isActive={isModEditActive}
		bind:isSavePromptActive
		bind:isDeletePromptActive
		on:change={() => {
			console.log('change', items);
		}}
		on:save={() => {
			console.log('save');
		}}
	/>
	<Account bind:isActive={isMenuActives[0]} />
	<Variables bind:isActive={isMenuActives[1]} />
	<Settings bind:isActive={isMenuActives[2]} />
</div>

<style lang="postcss">
	.component {
		height: 100%;
		background: var(--colour-background-primary);

		padding: 0 var(--padding) var(--padding) var(--padding);
		box-sizing: border-box;

		&.lock {
			touch-action: none;
		}

		& > .modules {
			display: flex;
			flex-direction: column;
			gap: 16px;
		}
	}
</style>
