<script lang="ts">
	import { twColors } from '$lib/data/tailwind-colors';
	import type { Color } from './types/color.js';

	type ColorWithSelectedShade = Color & {
		selectedShade: {
			number: string;
			hex: string;
		};
		classNames: {
			[key: string]: string;
		};
		locked: boolean;
	};

	function getRandomShades(
		inputArray: Color[],
		numberOfColors: number = 5
	): ColorWithSelectedShade[] {
		const randomColors: ColorWithSelectedShade[] = [];
		const classes = [
			'text',
			'bg',
			'decoration',
			'border',
			'ring-offset',
			'ring',
			'outline',
			'divide',
			'caret',
			'accent',
			'shadow'
		];

		for (let i = 0; i < numberOfColors; i++) {
			const randomColorIndex: number = Math.floor(Math.random() * inputArray.length);
			const randomColor: Color = inputArray[randomColorIndex];

			const randomShadeIndex: number = Math.floor(Math.random() * randomColor.shades.length);
			const randomShade = randomColor.shades[randomShadeIndex];

			const colorWithSelectedShade: ColorWithSelectedShade = {
				...randomColor,
				selectedShade: randomShade,
				classNames: {},
				locked: false
			};

			classes.forEach((className) => {
				colorWithSelectedShade.classNames[
					className
				] = `${className}-${randomColor.name}-${randomShade.number}`;
			});

			randomColors.push(colorWithSelectedShade);
		}

		console.log(randomColors);
		return randomColors;
	}

	let colors = getRandomShades(twColors);

	function generatePalette() {
		colors = colors.map((color) => {
			if (color.locked) {
				return color;
			}

			const randomColor = getRandomShades(twColors, 1)[0];
			return randomColor;
		});
	}

	function toggleLock(index: number) {
		colors[index].locked = !colors[index].locked;
	}
</script>

<div class="min-h-screen flex flex-col items-center justify-center bg-gray-100">
	<button
		class="mb-8 px-4 py-2 text-white bg-blue-600 rounded hover:bg-blue-700"
		on:click={generatePalette}
	>
		Generate Palette
	</button>

	<div class="flex">
		{#each colors as color, index}
			<div
				style={`background-color: ${color.selectedShade.hex}`}
				class={`w-64 h-64 m-2 rounded ${!color.locked && 'shadow-2xl'}`}
				on:click={() => toggleLock(index)}
				on:keypress={() => toggleLock(index)}
			>
				{color.name}: {color.selectedShade.hex}
				{color.locked ? '✓' : ''}
			</div>
		{/each}
	</div>
</div>
