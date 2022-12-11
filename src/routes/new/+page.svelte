<script lang="ts">
	import { goto } from '$app/navigation';
	import {
		ic_account_circle,
		ic_add_circle_outline,
		ic_bluetooth,
		ic_close,
		ic_delete,
		ic_edit,
		ic_emoji_symbols,
		ic_media_bluetooth_on,
		ic_pending,
		ic_person,
		ic_settings,
		ic_settings_remote,
		ic_shopping_cart,
	} from 'maic/two_tone';
	import SvgButton from '../../components/composable/buttons/SvgButton.svelte';
	import Dialog from '../../components/composable/Dialog.svelte';
	import Hint from '../../components/composable/Hint.svelte';
	import Input from '../../components/composable/Input.svelte';
	import Spacer from '../../components/composable/Spacer.svelte';
	import Account from '../../components/composed/Account.svelte';
	import Mod from '../../components/composed/Mod.svelte';
	import type { IMod } from '../../components/composed/mod/IMod';
	import ModEdit from '../../components/composed/ModEdit.svelte';
	import { toppingsW } from '../../components/composed/nav/TopNav.svelte';
	import Settings from '../../components/composed/Settings.svelte';
	import Variables from '../../components/composed/Variables.svelte';
	import { dropIn } from '../../core/transitioner/Transitioner';

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

	let isInputHintActive = false;
	let value = '';

	let edit: IMod | undefined;
	let isSavePromptActive = false;
	let isDeletePromptActive = false;
	let isEditSheetActive = false;

	let i = 0;

	$: suggestion = value
		? // eslint-disable-next-line @typescript-eslint/consistent-type-assertions
		  ({
				name: 'open sesame',
				whens: [
					{
						icon: ic_bluetooth,
						name: 'connected — "bluetooth_device"',
					},
				],
				dos: [
					{
						icon: ic_settings_remote,
						name: 'activate switch — "door"',
					},
				],
				buttons: [
					{
						icon: ic_edit,
						action() {
							edit = suggestion!;
							setTimeout(() => {
								isEditSheetActive = true;
							});
						},
					},
				],
		  } as IMod)
		: undefined;
	$: browse = value
		? new Array(3).fill(undefined).map<IMod>(() => ({
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
		  }))
		: [];
</script>

<div type="/new" class="component">
	<div class="input" in:dropIn={{ delay: 100 * ++i }}>
		<Input
			isMultiline
			height={168}
			bind:value
			label="describe your module"
		/>
		<div class="hint">
			<div class="icon">
				<Hint
					overrideColour="--colour-text-secondary"
					on:click={() => {
						isInputHintActive = true;
					}}
				/>
			</div>
			<div class="dialog">
				<Dialog bind:isActive={isInputHintActive} let:dismiss>
					<div slot="buttons" class="buttons">
						<SvgButton
							svg={ic_close}
							backgroundColour="--colour-background-primary"
							hoverBackgroundColour="--colour-background-secondary"
							on:click={dismiss}
						/>
					</div>
					<div class="content">
						<p>
							describe how you'd like your module to function!<br
							/><br />
							we'll use an AI to analyse it & provide you with a suggested
							result.
						</p>
					</div>
				</Dialog>
			</div>
		</div>
	</div>
	{#if suggestion}
		<p in:dropIn>inferred:</p>
		<div in:dropIn={{ delay: 100 }}>
			<Mod item={suggestion} height={300} isExpanded />
		</div>
	{/if}
	{#if browse.length > 0}
		<p in:dropIn={{ delay: 200 }}>from browse:</p>
		{#each browse as item, i}
			<div in:dropIn={{ delay: (i + 3) * 100 }}><Mod {item} /></div>
		{/each}
	{/if}
	<Spacer height={12} />
	<hr in:dropIn={{ delay: 100 * ++i }} />
	<Spacer height={12} />
	<div class="empty" in:dropIn={{ delay: 100 * ++i }}>
		<SvgButton
			svg={ic_add_circle_outline}
			width="100%"
			backgroundColour={value
				? '--colour-background-secondary'
				: '--colour-accent-secondary'}
			hoverBackgroundColour={value
				? '--colour-background-primary'
				: '--colour-background-secondary'}
			textColour={value
				? '--colour-text-primary'
				: '--colour-accent-primary'}
			hoverTextColour={value
				? '--colour-text-primary'
				: '--colour-accent-primary'}
			svgColour={value
				? '--colour-text-primary'
				: '--colour-accent-primary'}
			hoverSvgColour={value
				? '--colour-text-primary'
				: '--colour-accent-primary'}
			on:click={() => {
				edit = {
					name: 'untitled',
					whens: [],
					dos: [],
					buttons: [],
				};

				setTimeout(() => {
					isEditSheetActive = true;
				});
			}}>empty</SvgButton
		>
	</div>
	<Spacer height={24} />
	{#if edit}
		<ModEdit
			mod={edit}
			bind:isActive={isEditSheetActive}
			bind:isSavePromptActive
			bind:isDeletePromptActive
		/>
	{/if}
	<Account bind:isActive={isMenuActives[0]} />
	<Variables bind:isActive={isMenuActives[1]} />
	<Settings bind:isActive={isMenuActives[2]} />
</div>

<style lang="postcss">
	.component {
		position: relative;
		padding: 0 var(--padding) var(--padding) var(--padding);

		display: flex;
		flex-direction: column;
		gap: 12px;

		& > p {
			margin: 0;
			margin-top: 12px;
		}

		& > .input {
			height: 100%;
			width: 100%;

			& > .hint {
				position: relative;
				height: 24px;

				& > .icon {
					position: absolute;
					right: 0;
				}

				& > .dialog {
					& .buttons {
						display: flex;
					}

					& .content {
						padding: var(--padding);

						& > p {
							margin: 0;
						}
					}
				}
			}
		}

		& > .empty {
			display: flex;
			justify-content: center;
		}
	}
</style>
