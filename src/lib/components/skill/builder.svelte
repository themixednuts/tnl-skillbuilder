<script lang="ts">
	import * as Tabs from '$lib/shadcn/ui/tabs';
	import SpecializationGrid from './specializationgrid.svelte';
	let data = $state(
		new Array(12).fill(0).map((_, idx) => ({
			name: `Skill Name ${idx + 1}`,
			level: Math.max(Math.floor(Math.random() * 15), 1),
			src: 'https://placehold.co/400',
			specializations: new Array(Math.max(Math.floor(Math.random() * 6), 1))
				.fill(0)
				.map((_, idx) => ({
					id: `${idx}`,
					active: Math.random() > 0.5,
					src: 'https://placehold.co/400',
					cost: Math.floor(Math.max(Math.random() * 15, 5)),
					name: `Spec ${idx + 1}`,
					desc: `Description for skill spec ${idx + 1}`
				}))
		}))
	);
	let points = $state(100);
	$inspect(data);
</script>

<div class="container">
	<Tabs.Root value="skills">
		<Tabs.List>
			<Tabs.Trigger value="skills">Skills</Tabs.Trigger>
			<Tabs.Trigger value="specialization">Specialization</Tabs.Trigger>
		</Tabs.List>
		<Tabs.Content value="skills">Skills Tab</Tabs.Content>
		<Tabs.Content value="specialization">
			<SpecializationGrid bind:data bind:points></SpecializationGrid>
		</Tabs.Content>
	</Tabs.Root>
</div>
