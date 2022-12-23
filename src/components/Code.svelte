<script lang="ts">
	import { defaultControls } from '$lib/controls'
	import { draggable } from '@neodrag/svelte'
	import { controls } from '$lib'
	import Copy from './Copy.svelte'

	const defaults = JSON.parse(JSON.stringify(defaultControls))

	$: options = Object.entries($controls)
		.filter(([key]) => {
			if (['theme', 'width', 'height'].includes(key)) return false

			const isDefaultValue = $controls[key].value !== defaults[key].value

			return isDefaultValue
		})
		.map(
			([k, v]) => `\n\t&nbsp;&nbsp;<span class="options">${k}</span><span class="curlyboi">:</span><span class="value"> ${v.value}</span>`,
		)
		.join('')

	$: styleProps = ['width', 'height']
		.filter((prop) => {
			return $controls[prop].value !== defaults[prop].value
		})
		.map((prop) => {
			return `\n\t<span class="styleprop">--width</span><span class="operator">=</span><span class="string">"${$controls[prop].value}px"</span>`
		})

	$: propLength = options.length + styleProps.length

	let code = ''

	$: if ($controls) {
		code = `<span class="element">&lt;Keyboard</span>${
			options.length
				? `\n\t<span class="options">options</span><span class="curlyboi">=\{</span>${options}${
						options.length ? '\n\t' : ''
				  }<span class="curlyboi">\}</span>`
				: ''
		}${styleProps}${propLength > 0 ? '\n' : ' '}<span class="element">\/\></span>`
	}
</script>

<pre use:draggable><span><Copy>{@html code}</Copy></span></pre>

<style lang="scss">
	pre {
		position: fixed;
		inset: 0;
		top: unset;
		bottom: 20%;

		z-index: 2;

		width: fit-content;
		margin: auto;
		padding: 1rem 2rem;

		font-size: var(--font-sm);
		background: rgba(var(--light-a-rgb), 0.9);
		color: var(--dark-d);
		border-radius: var(--radius);
		backdrop-filter: blur(10px);
		box-shadow: var(--shadow);

		cursor: grab;

		&:active,
		&:focus {
			cursor: grabbing;
		}

		span {
			cursor: pointer;

			:global(.copy) {
				background-color: transparent;
				background: transparent;
			}
		}
	}

	:global(.element) {
		color: rgb(239, 96, 43);
	}
	:global(.styleprop) {
		color: var(--brand-a);
	}
	:global(.operator) {
		color: rgb(51, 71, 88);
	}
	:global(.string) {
		color: rgb(176 171 96);
	}
	:global(.curlyboi) {
		color: rgb(51, 71, 88);
	}
	:global(.options) {
		color: var(--dark-d);
	}
	:global(.value) {
		color: rgb(152, 96, 176);
	}
</style>