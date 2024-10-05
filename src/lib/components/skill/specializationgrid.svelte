<script lang="ts">
	import { Badge } from '$lib/shadcn/ui/badge';
	import { Separator } from '$lib/shadcn/ui/separator';
	import { cn } from '$lib/utils';
	import Icon from '../icon.svelte';
	import Specialization from './specialization.svelte';

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
		}[];
		points: number;
	};
	let { data: props = $bindable(), points = $bindable(100) }: Props = $props();
	let totalCost = $derived(
		props
			.map((skill) =>
				skill.specializations
					.map((spec) => (spec.active ? spec.cost : 0))
					.reduce((acc, curr) => acc + curr, 0)
			)
			.reduce((acc, curr) => acc + curr, 0)
	);

	let activeSkill: Props['data'][number] = $state(props[0]);
</script>

<div class="flex place-content-center gap-2">
	<Badge variant="secondary" class="min-w-28 gap-2 rounded-lg p-1">
		<img src="https://placehold.co/400" alt="" class="size-8" />
		<div class="grow text-end">{totalCost}</div>
		<Separator orientation="vertical" />
		<div class="">{points}</div>
	</Badge>
	<button
		onclick={() => {
			props.forEach((_, idx, arr) =>
				arr[idx].specializations.forEach((_, idx, arr) => (arr[idx].active = false))
			);
		}}
	>
		<Badge variant="destructive" class="size-full rounded-lg">Reset</Badge>
	</button>
</div>
<div class="flex flex-wrap place-content-center gap-2 xl:flex-nowrap">
	<div class="grid w-max min-w-max grid-cols-2 gap-2 p-2">
		{#each props as _, i}
			<button
				class="cursor-default"
				onclick={() => {
					activeSkill = props[i];
				}}
			>
				<Specialization
					bind:data={props[i]}
					disabledFn={(cost: number) => cost + totalCost >= points}
				/>
			</button>
		{/each}
	</div>
	<div class="flex max-w-[48rem] flex-col">
		<div class="text-center">
			{activeSkill.name}
		</div>
		<div class="flex flex-col gap-2">
			{#each activeSkill.specializations as { src, id, active, name, desc, cost }, i}
				<button
					class={cn('flex place-items-center gap-2 rounded-l-full border-2 border-gray-400 p-2', {
						'border-purple-400': active
					})}
					onclick={() => (activeSkill.specializations[i].active = !active)}
					disabled={totalCost >= points && !active}
				>
					<Icon
						type="skill"
						variant="circle"
						class={cn('size-24', { 'opacity-40': !active })}
						{src}
					/>
					<div class="flex h-24 flex-col place-items-start gap-2">
						<div class="flex gap-2">
							<div class="text-start">{name}</div>
							<Badge variant="secondary" class="w-12 rounded p-1">
								<img src="https://placehold.co/400" alt="" class="mr-2 size-4" />
								{cost}
							</Badge>
						</div>
						<div class="overflow-auto text-start">
							{desc} Dwelling and speedily ignorant any steepest. Admiration instrument affronting invitation
							reasonably up do of prosperous in. Shy saw declared age debating ecstatic man. Call in
							so want pure rank am dear were. Remarkably to continuing in surrounded diminution on. In
							unfeeling existence objection immediate repulsive on he in. Imprudence comparison uncommonly
							me he difficulty diminution resolution. Likewise proposal differed scarcely dwelling as
							on raillery. September few dependent extremity own continued and ten prevailed attending.
							Early to weeks we could.
						</div>
					</div>
				</button>
			{/each}
		</div>
	</div>
</div>
