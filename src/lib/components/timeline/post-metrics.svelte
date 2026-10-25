<script lang="ts">
	import type { Component } from 'svelte';

	import type { AppBskyFeedDefs } from '@atcute/bluesky';

	import { formatCompactNumber, formatLongNumber } from '$lib/utils/intl/number';

	import ArrowsRepeatRightLeftOutlined from '$lib/components/central-icons/arrows-repeat-right-left-outlined.svelte';
	import Bubble_2Outlined from '$lib/components/central-icons/bubble-2-outlined.svelte';
	import HeartOutlined from '$lib/components/central-icons/heart-outlined.svelte';
	import BookmarkOutlined from '../central-icons/bookmark-outlined.svelte';
	import QuoteOutlined from '../central-icons/quote-outlined.svelte';

	interface Props {
		post: AppBskyFeedDefs.PostView;
		postUrl: string;
	}

	const { post, postUrl }: Props = $props();

	const replyCount = $derived(post.replyCount || 0);
	const likeCount = $derived(post.likeCount || 0);
	const repostCount = $derived((post.repostCount || 0) + (post.quoteCount || 0));
	const quoteCount = $derived(post.quoteCount || 0);
	const bookmarkCount = $derived(post.bookmarkCount || 0);
</script>

{#snippet Stat(count: number, Icon: Component, one: string, many: string, page: string | null)}
	<div
		title={count === 1 ? `${formatLongNumber(count)} ${one}` : `${formatLongNumber(count)} ${many}`}
		class="stat"
	>
		<div class="icon">
			<Icon />
		</div>

		{#if page != null}
			<a class="count" href="{postUrl}/{page}">
				{formatCompactNumber(count)}
			</a>
		{:else}
			<span class="count">
				{formatCompactNumber(count)}
			</span>
		{/if}
	</div>
{/snippet}

<div class="post-metrics">
	{@render Stat(replyCount, Bubble_2Outlined, 'reply', 'replies', '')}
	{@render Stat(repostCount, ArrowsRepeatRightLeftOutlined, 'repost', 'reposts', 'reposts')}
	{@render Stat(quoteCount, QuoteOutlined, 'quote', 'quotes', 'quotes')}
	{@render Stat(likeCount, HeartOutlined, 'like', 'likes', 'likes')}
	{@render Stat(bookmarkCount, BookmarkOutlined, 'bookmark', 'bookmarks', null)}
</div>

<style>
	.post-metrics {
		z-index: 1;
		position: relative;
		width: fit-content;
		display: flex;
		align-items: center;
		gap: 10px;
		margin-top: 12px;
		color: var(--text-blurb);
	}

	.icon {
		display: grid;
		place-items: center;
		font-size: 14px;
	}

	.stat {
		display: flex;
		align-items: center;
		gap: 4px;
		min-width: 0px;
		max-width: 100%;
	}

	.count {
		padding-right: 8px;
		overflow: hidden;
		font-size: 0.75rem;
		line-height: 1.25rem;
		text-overflow: ellipsis;
		white-space: nowrap;
		color: inherit;
	}
</style>
