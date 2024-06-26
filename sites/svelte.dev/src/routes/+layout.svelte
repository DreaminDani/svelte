<script>
	import '@sveltejs/site-kit/styles/index.css';

	import { injectSpeedInsights } from '@vercel/speed-insights/sveltekit';
	import { browser } from '$app/environment';
	import { page } from '$app/stores';
	import { Icon, Shell, Banners } from '@sveltejs/site-kit/components';
	import { Nav, Separator } from '@sveltejs/site-kit/nav';
	import { Search, SearchBox } from '@sveltejs/site-kit/search';

	injectSpeedInsights();

	export let data;

	/** @type {import('@sveltejs/kit').Snapshot<number>} */
	let shell_snapshot;

	export const snapshot = {
		capture() {
			return {
				shell: shell_snapshot?.capture()
			};
		},
		restore(data) {
			shell_snapshot?.restore(data.shell);
		}
	};
</script>

<svelte:head>
	{#if !$page.route.id?.startsWith('/blog/')}
		<meta name="twitter:card" content="summary" />
		<meta name="twitter:image" content="https://svelte.dev/images/twitter-thumbnail.jpg" />
		<meta name="og:image" content="https://svelte.dev/images/twitter-thumbnail.jpg" />
	{/if}
</svelte:head>

<div style:display={$page.url.pathname !== '/docs' ? 'contents' : 'none'}>
	<Shell
		nav_visible={$page.route.id !== '/(authed)/repl/[id]/embed'}
		bind:snapshot={shell_snapshot}
	>
		<Nav slot="top-nav" title={data.nav_title} links={data.nav_links}>
			<svelte:fragment slot="home-large">
				<strong>svelte</strong>.dev
			</svelte:fragment>

			<svelte:fragment slot="home-small">
				<strong>svelte</strong>
			</svelte:fragment>

			<svelte:fragment slot="search">
				{#if $page.url.pathname !== '/search'}
					<Search />
				{/if}
			</svelte:fragment>

			<svelte:fragment slot="external-links">
				<a href="https://learn.svelte.dev/">Tutorial</a>

				<a href="https://kit.svelte.dev">SvelteKit</a>

				<Separator />

				<a href="/chat" title="Discord Chat">
					<span class="small">Discord</span>
					<span class="large"><Icon name="discord" /></span>
				</a>

				<a href="https://github.com/sveltejs/svelte" title="GitHub Repo">
					<span class="small">GitHub</span>
					<span class="large"><Icon name="github" /></span>
				</a>
			</svelte:fragment>
		</Nav>

		<slot />

		<Banners slot="banner-bottom" data={data.banner} />
	</Shell>
</div>

{#if browser}
	<SearchBox />
{/if}

<style>
	:global(:root) {
		color-scheme: light dark;
	}

	:global(html, body) {
		height: 100%;
		width: 100%;
	}
</style>
