<script lang="ts">
	import Error from '$components/Error.svelte'
	import { page } from '$app/stores'
	import { DEV } from 'esm-env'

	if (DEV) console.log($page.error)
</script>

<template lang="pug">

	+if('DEV && $page.status === "404"')
		.four-o-four
			Error
		
		+else
			h1 {$page.status}

	.error
		pre.message {$page.error.message}
		pre.stack {$page.error.stack?.split($page.error.message)[1]}

</template>

<a href="/">go back</a>

<slot />

<style lang="scss">
	a {
		display: flex;

		width: max-content;
		margin: 0 auto 0 auto;

		font-size: 1.5rem;
		text-align: center;

		color: var(--dark-a, slategray);
		text-decoration-skip-ink: auto;
		text-decoration-color: #ffffff50;
	}

	h1 {
		margin-top: 5vh;

		color: var(--warn);

		font-size: 10rem;
		font-weight: 100;
		text-align: center;
	}

	.four-o-four {
		width: fit-content;
		width: 40rem;
		margin: auto;
	}

	.error {
		display: flex;
		flex-direction: column;
		align-items: center;

		margin-top: 5vh;
		text-align: center;

		pre {
			max-width: 90vw;

			text-align: left;
			line-height: 1.5rem;
		}

		.message {
			width: max-content;
			height: max-content;
			margin: 1rem auto;
			padding: 1rem;

			color: var(--dark-d);
			background: transparent;
			border: 1px solid var(--light-d);
			border-radius: var(--radius-lg);
		}

		.stack {
			color: rgba(var(--dark-d-rgb), 0.5);
			max-height: 40vh;
			overflow-y: auto;
		}

		::-webkit-scrollbar {
			background-color: var(--light-a);
			width: 10px;
			height: 10px;
		}
		::-webkit-scrollbar-thumb {
			background-color: rgba(var(--light-d-rgb), 0.5);
			border-radius: 5px;
		}
		::-webkit-scrollbar-track {
			background-color: rgba(var(--light-d-rgb), 0.1);
		}
		::-webkit-scrollbar-corner {
			background-color: rgba(var(--light-d-rgb), 0.1);
		}
	}
</style>
