<script lang="ts">
	// Types
	import type { CssClasses } from '../../index.js';
	import type { Action } from 'svelte/action';

	// Props (initials)
	/** Initials only - Provide up to two text characters. */
	export let initials = '';
	/** Initials only - Provide classes to set the SVG text fill color. */
	export let fill: CssClasses = 'fill-token';
	/** Initials only - Set the base font size for the scalable SVG text. */
	export let fontSize = 150;

	// Props (actions)
	/** Provide the avatar image element source. */
	export let src = '';
	/** Provide the fallback image element source. */
	export let fallback = '';
	/**
	 * Image only. Provide a Svelte action reference, such as `filter`.
	 */
	export let action: Action<HTMLElement, string> = () => {};
	/** Image only. Provide Svelte action params, such as Apollo. */
	export let actionParams = '';

	// Props (styles)
	/** Provide classes to set background styles. */
	export let background: CssClasses = 'bg-surface-400-500-token';
	/** Provide classes to set avatar width. */
	export let width: CssClasses = 'w-16';
	/** Provide classes to set border styles. */
	export let border: CssClasses = '';
	/** Provide classes to set rounded style. */
	export let rounded: CssClasses = 'rounded-full';
	/** Provide classes to set shadow styles. */
	export let shadow: CssClasses = '';
	/** Provide classes to set cursor styles. */
	export let cursor: CssClasses = '';

	// Base Classes
	let cBase = 'flex aspect-square text-surface-50 font-semibold justify-center items-center overflow-hidden isolate';
	let cImage = 'w-full object-cover';

	// Reactive Classes
	$: classesBase = `${cBase} ${background} ${width} ${border} ${rounded} ${shadow} ${cursor} ${$$props.class ?? ''}`;

	// Utility Functions
	function prunedRestProps() {
		delete $$restProps.class;
		return $$restProps;
	}
</script>

<!-- FIXME: resolve a11y warnings -->
<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
<figure class="avatar {classesBase}" data-testid="avatar" {...prunedRestProps()} on:click on:keydown on:keyup on:keypress>
	{#if src || fallback}
		<img
			class="avatar-image {cImage}"
			style={$$props.style ?? ''}
			{src}
			alt={$$props.alt || ''}
			use:action={actionParams}
			on:error={() => (src = fallback)}
		/>
	{:else if initials}
		<svg class="avatar-initials w-full h-full" viewBox="0 0 512 512">
			<text
				x="50%"
				y="50%"
				dominant-baseline="central"
				text-anchor="middle"
				font-weight="bold"
				font-size={fontSize}
				class="avatar-text {fill}"
			>
				{String(initials).substring(0, 2).toUpperCase()}
			</text>
		</svg>
	{:else}
		<slot />
	{/if}
</figure>
