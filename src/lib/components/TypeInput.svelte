<script lang="ts">
	// actual typing practice code
	export let text = 'Lorem ipsum\nHello World!';
	export let onComplete: () => void = () => {};

	const newlineConstant = '↩';

	const preprocess = (text: string): string[] => {
		// split by new lines
		const lines = text.split('\n').map((line) => line + newlineConstant);
		return [...lines.slice(0, -1), lines[lines.length - 1].slice(0, -1)];
	};

	const lines = preprocess(text);
	let index = 0;

	const lineAndCursorFromIndex = (index: number) => {
		// get the line from the cursor based on number of characters on each line

		let sum = 0;
		for (let i = 0; i < lines.length; i++) {
			sum += lines[i].length;
			if (sum > index) {
				return [i, index - (sum - lines[i].length)];
			}
		}
		return [0, 0];
	};
	$: [curLine, cursor] = lineAndCursorFromIndex(index);

	const handleKeyDown = (e: KeyboardEvent) => {
		// only catch A-Z, a-z, space, enter (ignore rest)
		if (['Shift', 'Meta'].includes(e.key)) return;
		// prevent default
		e.preventDefault();

		// verify if the key pressed is the same as the character at the cursor
		if (
			(e.key === 'Enter' && lines[curLine][cursor] === newlineConstant) ||
			e.key === lines[curLine][cursor]
		) {
			// if it is, move the cursor forward
			index++;

			if (index >= text.length) {
				onComplete();
			}
		} else {
			// if it isn't, pass
		}
	};
</script>

<!-- svelte-ignore a11y-no-noninteractive-tabindex -->
<div class="mockup-code focus:shadow-md" tabindex="0" on:keydown={handleKeyDown}>
	<!-- <pre data-prefix="$"><code>npm i daisyui</code></pre> -->
	{#each lines as line, i}
		{#if i == curLine}
			<pre data-prefix=">"><code class="align-baseline"
					>{line.slice(0, cursor)}<span class="underline">{line[cursor]}</span>{line.slice(
						cursor + 1
					)}</code
				></pre>
		{:else}
			<pre data-prefix="$"><code>{line}</code></pre>
		{/if}
	{/each}
</div>
