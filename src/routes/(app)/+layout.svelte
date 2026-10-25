<script lang="ts">
	import type { Snippet } from 'svelte';

	import { provideIsland } from '$lib/components/island.svelte';

	import { PUBLIC_APP_NAME } from '$env/static/public';

	import '$lib/styles/app.css';
	import MagnifyingGlassOutlined from '$lib/components/central-icons/magnifying-glass-outlined.svelte';
	import CircleInfoOutlined from '$lib/components/central-icons/circle-info-outlined.svelte';

	interface Props {
		children: Snippet<[]>;
	}

	const { children }: Props = $props();

	provideIsland();
</script>

<div class="navbar">
	<div class="navbar-contents">
		<div class="navbar-left">
			<a class="navbar-title" href="/">{PUBLIC_APP_NAME}</a>
		</div>
		<div class="navbar-right">
			<form method="get" action="/search" class="navbar-search">
				<input
					type="search"
					name="q"
					required
					placeholder="Search"
					class="input"
				/>

				<button tabindex={-1} aria-label="Search" type="submit" class="button">
					<MagnifyingGlassOutlined />
				</button>
			</form>
			<a class="navbar-about-link navbar-icon" href="/about">
				<CircleInfoOutlined />
			</a>
		</div>
	</div>
</div>

{@render children()}

<style>
	.navbar {
		background: var(--bg-primary);
		padding: 0 12px;
	}

	.navbar-contents {
		display: flex;
		justify-content: space-between;
		align-items: center;
		max-width: 900px;
		margin: 0 auto;
		padding: 1em 0;
	}

	.navbar-title {
		font-weight: bold;
	}

	.navbar-right {
		display: flex;
  		align-items: center;
	}

	.navbar-icon {
		line-height: 0px;
  		font-size: 20px;
		margin-left: 8px;
	}

	.navbar-search {
		display: flex;

		& .input, .button {
			border: 1px solid var(--divider-md);
			color: inherit;
			background: var(--bg-slate);
			padding: 0.2em;
		}

		& .input::placeholder {
			color: var(--text-blurb);
			font-style: italic;
		}

		& .button {
			display: inline-grid;
			place-items: center;
			background: var(--bg-primary);
		}
	}
</style>