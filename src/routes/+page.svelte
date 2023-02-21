<script lang="ts">
	import TypeInput from '@/lib/components/TypeInput.svelte';
</script>

<div class="">
	<div class="mx-auto">
		<TypeInput
			onComplete={() => console.log('yay')}
			text={'This is a typing test.\nOnly for personal use.'}
		/>
	</div>
</div>
