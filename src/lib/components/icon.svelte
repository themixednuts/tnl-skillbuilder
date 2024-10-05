<script lang="ts">
	import type { HTMLImgAttributes } from 'svelte/elements';
	import { cn } from '$lib/utils';

	type Props = {
		variant?: 'circle' | 'square' | 'hexagon';
		level?: number;
		type: 'item' | 'skill';
	} & HTMLImgAttributes;

	let { variant = 'square', level = $bindable(), type = 'item', ...rest }: Props = $props();

	function wheelUp(delta: number) {
		return delta < 0;
	}

	const isItem = $derived(type === 'item');

	const tier = $derived(level ? Math.floor((level - 1) / 5) : undefined);
</script>

<div
	class={cn(`grid aspect-square size-24 overflow-clip border-4 border-gray-400 ${rest.class}`, {
		'border-green-400': isItem ? tier === 1 : tier === 0,
		'border-blue-400': isItem ? tier === 2 : tier === 1,
		'border-purple-400': isItem ? tier === 3 : tier === 2,
		'rounded-full': variant === 'circle'
		// 'border-transparent [clip-path:polygon(50%_0,100%_25%,100%_75%,50%_100%,0_75%,0_25%)]':
		// 	variant === 'hexagon'
	})}
	onwheel={(e) => {
		if (level === undefined) return;
		e.preventDefault();
		if (wheelUp(e.deltaY)) {
			if (isItem ? level > 19 : level > 14) return;
			level++;
		} else {
			if (level < 2) return;
			level--;
		}
	}}
>
	<img src={rest.src} alt="" {...rest} class="col-start-1 row-start-1" />
	{#if level}
		<span
			class={cn(
				'col-start-1 row-start-1 mb-1 mr-1 size-6 self-end justify-self-end px-2 font-bold text-white [clip-path:polygon(50%_0,100%_25%,100%_75%,50%_100%,0_75%,0_25%)]',
				{
					'bg-green-400': isItem ? tier === 1 : tier === 0,
					'bg-blue-400': isItem ? tier === 2 : tier === 1,
					'bg-purple-400': isItem ? tier === 3 : tier === 2
				}
			)}>{((level - 1) % 5) + 1}</span
		>
	{/if}
</div>
