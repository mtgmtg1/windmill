<script lang="ts">
	import type { AppInput } from '../../inputType'

	export let input: AppInput
</script>

{#if input.type === 'connected'}
	<div class="flex gap-2 flex-col">
		<div>This component is expecting a connected input: </div>
		<div>
			<code class="bg-gray-50 border text-tertiary">{input.connection?.path} </code>
		</div>
	</div>
{/if}
