<script lang="ts">
	import {
		ic_account_circle,
		ic_bluetooth,
		ic_delete,
		ic_download,
		ic_emoji_symbols,
		ic_media_bluetooth_on,
		ic_pending,
		ic_person,
		ic_settings,
		ic_settings_remote,
		ic_shopping_cart,
	} from 'maic/two_tone';
	import Spacer from '../../components/composable/Spacer.svelte';
	import Svg from '../../components/composable/Svg.svelte';
	import Mod from '../../components/composed/Mod.svelte';
	import type { IMod } from '../../components/composed/mod/IMod';
	import { dropIn } from '../../core/transitioner/Transitioner';
	import { toppingsW } from '../../components/composed/nav/TopNav.svelte';
	import { goto } from '$app/navigation';
	import Variables from '../../components/composed/Variables.svelte';
	import Account from '../../components/composed/Account.svelte';
	import Settings from '../../components/composed/Settings.svelte';

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

	const isMenuActives = [false, false, false];

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
				icon: ic_person,
				action() {},
			},
			{
				icon: ic_shopping_cart,
				action() {},
			},
		],
	}));
</script>

<div type="/browse" class="component">
	<div class="modules">
		<Mod
			item={{
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
						icon: ic_person,
						action() {},
					},
					{
						icon: ic_shopping_cart,
						action() {},
					},
				],
			}}
		>
			<div slot="name" class="heading" let:item>
				<p class="featured">FEATURED</p>
				<h1>{item.name}</h1>
				<p class="user">user</p>
				<Spacer height={40} />
				<div class="downloads">
					<Svg svg={ic_download} colour="--colour-text-secondary" />
					10k
				</div>
			</div>
		</Mod>
		{#each items as item, i}
			<div in:dropIn={{ delay: i * 100 }}>
				<Mod {item} />
			</div>
		{/each}
	</div>
	<Account bind:isActive={isMenuActives[0]} />
	<Variables bind:isActive={isMenuActives[1]} />
	<Settings bind:isActive={isMenuActives[2]} />
</div>

<style lang="postcss">
	.component {
		padding: 0 var(--padding) var(--padding) var(--padding);

		& > .modules {
			display: flex;
			flex-direction: column;
			gap: 16px;

			& .heading {
				& > .featured {
					text-transform: uppercase;
					font-weight: 700;
					margin: 0;
					margin-top: 8px;
					margin-left: -20px;
					color: var(--colour-text-secondary);
					font-size: 0.8em;
					/* text-decoration: underline; */
				}

				& > h1 {
					color: var(--colour-text-primary);
					margin-left: 8px;
					margin-bottom: 24px;
				}

				& > .user {
					/* text-align: right; */
					margin-left: 16px;
				}

				& > .downloads {
					display: flex;
					align-items: center;
					gap: 4px;
					margin-left: -20px;
					margin-bottom: 8px;
					font-weight: 700;
					color: var(--colour-text-secondary);
				}
			}
		}
	}
</style>
