<script lang="ts" module>
	const DEFAULT_RATIO = { width: 16, height: 9 };
</script>

<script lang="ts">
	import type { AppBskyEmbedGallery } from '@atcute/bluesky';

	import { trimRichText } from '$lib/utils/bluesky/richtext';

	import ImageAlt from './components/image-alt.svelte';

	interface Props {
		embed: AppBskyEmbedGallery.View;
		borderless?: boolean;
		standalone?: boolean;
		blur?: boolean;
	}

	const { embed, borderless, standalone, blur }: Props = $props();

	const images = $derived(embed.items);
	const length = $derived(images.length);
</script>

<div class={['gallery-embed', !borderless && 'is-bordered', standalone && length === 1 && 'is-aligned']}>
	{#each images as _, idx}
		<div class="gallery-item">
			{@render Image(idx)}
		</div>
	{/each}
</div>

{#snippet Image(index: number)}
	{@const image = images[index]}
	{@const alt = trimRichText(image.alt)}

	<a href={image.fullsize.replace('@jpeg', '@png')} target="_blank" rel="noopener" class="image-wrapper">
		<img loading="lazy" src={image.thumbnail} {alt} title="{alt}" class={`image` + (blur ? ` is-blurred` : ``)} />
	</a>

	<div class="item-info">
		<p class="item-counter">{index + 1}/{images.length}</p>
		{#if alt}<ImageAlt {alt} />{/if}
	</div>
{/snippet}

<style>
	.gallery-embed {
		width: 100%;
		display: flex;
  		height: 22em;
		overflow-x: scroll;
  		overflow-y: hidden;
  		flex-direction: row;
	}

	.gallery-item {
		margin: 4px;
	}

	.item-info {
		z-index: 1;
		position: relative;
		top: calc(-100% + 4px);
		left: 4px;
		display: flex;
		justify-content: end;
		align-items: center;
		width: fit-content;

		:global(& .button) {
			position: unset;
		}
	}

	.item-counter {
		appearance: none;
		opacity: 0.6;
		border: 0;
		border-radius: 4px;
		background: rgb(10 10 10);
		padding: 0 4px;
		height: 18px;
		color: #ffffff;
		font-weight: 700;
		font-size: 9px;
		letter-spacing: 0.05em;
		cursor: default;
	}

	.is-aligned {
		align-self: baseline;
		max-width: 100%;
	}

	.image-wrapper {
		inset: 0;
		outline: none;
	}
	.image {
		background: var(--bg-slate);
		height: 100%;
		object-fit: cover;
		font-size: 0px;
		border-radius: 6px;
	}
	.is-blurred {
		scale: 125%;
		filter: blur(24px);
	}

	.placeholder {
		width: 100vw;
		height: 100vh;
	}
</style>
