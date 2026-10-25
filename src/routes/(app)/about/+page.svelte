<script lang="ts">
	import { PUBLIC_APP_NAME, PUBLIC_SOURCE_URL } from '$env/static/public';
	import type { PageProps } from './$types';

    import PageContainer from '$lib/components/page/page-container.svelte';

	const { data }: PageProps = $props();
</script>

<PageContainer>
    <main class="about-page">
        <section>
            <h2 class="heading">About {PUBLIC_APP_NAME}</h2>
            <p>{PUBLIC_APP_NAME} is a JavaScript-optional web frontend that allows you to view Bluesky.</p><br>
            <p>
                It is based on <a href="https://github.com/mary-ext/anartia" target="_blank">Anartia</a> with a few additional improvements and updates.
                You can view the source code <a href="{PUBLIC_SOURCE_URL}" target="_blank">here</a>.
            </p>
        </section>
    </main>
</PageContainer>

<style>
    .about-page {
		background: var(--bg-primary);
		padding: 16px;
    }

    .heading {
		font-weight: 600;
		font-size: 1rem;
		line-height: 1.5rem;
	}
</style>