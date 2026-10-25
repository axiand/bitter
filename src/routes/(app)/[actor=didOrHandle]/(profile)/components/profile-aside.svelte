<script lang="ts">
	import type { AppBskyActorDefs, AppBskyActorLink } from '@atcute/bluesky';

	import { base } from '$app/paths';

	import { findLabel, FlagsBlurMedia } from '$lib/moderation';
	import { normalizeDisplayName } from '$lib/utils/bluesky/display';
	import { trimRichText } from '$lib/utils/bluesky/richtext';
	import { formatCompactNumber, formatLongNumber } from '$lib/utils/intl/number';

	import Avatar from '$lib/components/avatar.svelte';
	import SquareArrowTopRightOutlined from '$lib/components/central-icons/square-arrow-top-right-outlined.svelte';
	import OverflowMenu from '$lib/components/overflow-menu.svelte';
	import RichtextRawRenderer from '$lib/components/richtext-raw-renderer.svelte';

	interface Props {
		profile: AppBskyActorDefs.ProfileViewDetailed;
	}

	const { profile }: Props = $props();

	const did = $derived(profile.did);

	const description = trimRichText(profile.description ?? '');

	const blur = $derived(!!findLabel(profile.labels, profile.did, FlagsBlurMedia));
</script>

{#snippet Stat(count: number = 0, one: string, many: string, href: string)}
	{@const compact = formatCompactNumber(count)}
	{@const long = formatLongNumber(count)}

	<a
		href="{base}/{did}/{href}"
		title={compact !== long ? (count === 1 ? `${long} ${one}` : `${long} ${many}`) : ''}
		class="stat-entry"
	>
		<span class="stat-count">{compact}</span>
		<span> {count === 1 ? one : many}</span>
	</a>
{/snippet}

{#snippet ProfileLink(link: AppBskyActorDefs.ProfileLinkView)}
	{@const displayTitle = (link.title != null && link.title.length > 0) ? link.title : new URL(link.url).hostname}

	<a class="profile-link" href="{link.url}" target="_blank">
		{#if link.icon}
			<img class="profile-link-icon" src="{link.icon}" alt="Site icon for {displayTitle}">
		{/if}
		<p class="profile-link-title">{displayTitle}</p>
	</a>
{/snippet}

<div class="profile-aside">
	<Avatar {profile} size="xl" {blur} />

	<OverflowMenu
		slim={false}
		items={[
			{
				label: `Open in Bluesky app`,
				href: `https://bsky.app/profile/${profile.did}`,
				external: true,
				icon: SquareArrowTopRightOutlined,
			},
			{
				label: `Open in PDSls`,
				href: `https://pdsls.dev/at://${profile.did}`,
				external: true,
				icon: SquareArrowTopRightOutlined,
			},
		]}
	/>

	<div class="name-wrapper">
		<p dir="auto" class="display-name">
			{normalizeDisplayName(profile.displayName ?? '') || profile.handle.slice(0, 64)}
		</p>
		<p class="handle">@{profile.handle}</p>
	</div>

	{#if description}
		<RichtextRawRenderer text={description} />
	{/if}

	<div class="profile-links">
		{#each profile.links as link}
			{@render ProfileLink(link)}
		{/each}
	</div>

	<div class="stats">
		{@render Stat(profile.followersCount, 'follower', 'followers', 'followers')}
		{@render Stat(profile.followsCount, 'following', 'following', 'following')}
	</div>
</div>

<style>
	.profile-aside {
		display: flex;
		position: relative;
		flex-direction: column;
		gap: 8px;
		background: var(--bg-primary);
		padding: 16px;
		min-width: 0;

		:global(.overflow-menu) {
			position: absolute;
			top: 12px;
			right: 12px;
		}
	}

	.display-name {
		font-weight: 700;
		font-size: 1.25rem;
		line-height: 1.75rem;
		overflow-wrap: break-word;
	}
	.handle {
		color: var(--text-blurb);
		overflow-wrap: break-word;
	}

	.stats {
		display: flex;
		flex-wrap: wrap;
		gap: 20px;

		min-width: 0;
	}
	.stat-entry {
		color: var(--text-blurb);

		&:hover {
			text-decoration: underline;
		}
	}
	.stat-count {
		color: var(--text-primary);
		font-weight: 700;
	}

	.profile-links {
		display: inline-flex;
		flex-flow: wrap;
	}

	.profile-link {
		padding: 4px 10px;
		background: var(--divider-sm);
		margin-right: 8px;
		display: inline-flex;
		margin-bottom: 6px;
		border-radius: 999px;
		font-size: 12px;
		color: var(--text-primary);
		border: 1px solid var(--tap-md);
		align-items: center;
	}

	.profile-link-title {
		width: fit-content;
	}

	.profile-link-icon {
		height: 16px;
		width: 16px;
		margin-right: 6px;
	}
</style>
