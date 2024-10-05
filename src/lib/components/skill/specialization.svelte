<script lang="ts">
	import Icon from '$lib/components/icon.svelte';
	import { cn } from '$lib/utils';
	import type { HTMLAttributes } from 'svelte/elements';

	type Props = {
		data: {
			name: string;
			src: string;
			level: number;
			specializations: {
				id: string;
				src: string;
				active: boolean;
				cost: number;
				name: string;
				desc: string;
			}[];
		};
		disabledFn: (cost: number) => boolean;
	};

	let {
		data = $bindable(),
		class: className,
		disabledFn,
		...rest
	}: Props & HTMLAttributes<HTMLDivElement> = $props();
</script>

<div class={cn(`flex w-96 gap-x-2 border-4 p-2 ${className}`, {})}>
	<Icon bind:level={data.level} src={data.src} class="size-20" type="skill" />
	<div class="flex flex-col gap-2">
		<div class="self-start">{data.name}</div>
		<div class="flex grow flex-wrap place-items-center gap-1">
			{#each data.specializations as { src, active, cost }, i}
				<button
					class="rounded-full"
					disabled={disabledFn(cost) && !active}
					onclick={() => (data.specializations[i].active = !active)}
				>
					<Icon
						type="skill"
						variant="circle"
						class={cn('size-12 border-purple-400', { 'border-gray-400 opacity-40': !active })}
						{src}
					></Icon>
				</button>
			{/each}
		</div>
	</div>
</div>
