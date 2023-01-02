<script lang="ts">
	import { browser } from '$app/environment'
	import { page } from '$app/stores'
	import AppPreview from '$lib/components/apps/editor/AppPreview.svelte'
	import type { EditorBreakpoint } from '$lib/components/apps/types'

	import { Skeleton } from '$lib/components/common'
	import { WindmillIcon } from '$lib/components/icons'
	import { AppService, AppWithLastVersion, GlobalUserInfo, UserService } from '$lib/gen'
	import { writable } from 'svelte/store'

	let app: AppWithLastVersion | undefined = undefined
	let user: GlobalUserInfo | undefined = undefined

	async function loadApp() {
		app = await AppService.getPublicAppBySecret({
			workspace: $page.params.workspace,
			path: $page.params.secret
		})
	}

	async function loadUser() {
		try {
			user = await UserService.globalWhoami()
		} catch (e) {}
	}
	if (browser) {
		loadApp()
		loadUser()
	}

	const breakpoint = writable<EditorBreakpoint>('lg')
</script>

<div class="z-50 text-xs fixed bottom-1 right-2 ">
	<a href="https://windmill.dev" class="whitespace-nowrap text-gray-500 inline-flex items-center"
		>Powered by &nbsp;<WindmillIcon />&nbsp;Windmill</a
	>
</div>
{#if app}
	<div class="z-50 text-xs text-gray-500 fixed top-1 left-2">
		<div>
			{#if user}
				Logged in as {user.email}
			{:else}
				Not logged in
			{/if}
		</div>
		<a class="text-blue-400" href="/">Go to app</a>
	</div>
	<div class="border rounded-md p-2 w-full">
		<AppPreview
			workspace={$page.params.workspace}
			summary={app.summary}
			app={app.value}
			appPath={app.path}
			{breakpoint}
			policy={app.policy}
		/>
	</div>
{:else}
	<Skeleton layout={[[4], 0.5, [50]]} />
{/if}