<script lang="ts">
	import { onMount } from 'svelte';

	let canvas: HTMLCanvasElement;
	let drawing = false;
	let x = 0;
	let y = 0;
	let lastRect = {};
	let imageData: ImageData;
	let ctx: CanvasRenderingContext2D;

	const clearCanvas = () => {
		if (ctx) {
			ctx.reset();
		}
	};

	const onMouseDown = (e: MouseEvent) => {
		// When user clicks before releasing
		x = e.offsetX;
		y = e.offsetY;
		drawing = true;
		if (ctx) {
			imageData = ctx.getImageData(0, 0, 300, 150);
		}
	};

	const onMouseUp = (e: MouseEvent) => {
		if (drawing) {
			if (!ctx) return;
			drawRect(ctx, x, y, e.offsetX, e.offsetY);
			x = 0;
			y = 0;
			drawing = false;
		}
		// x = e.offsetX;
		// y = e.offsetY;
		drawing = false;
	};

	const onMouseMove = (e: MouseEvent) => {
		if (!drawing) return;
		if (!ctx) return;
		ctx.putImageData(imageData, 0, 0);
		drawRect(ctx, x, y, e.offsetX, e.offsetY);
		// x = e.offsetX;
		// y = e.offsetY;
	};

	const drawLine = (
		ctx: CanvasRenderingContext2D,
		x1: number,
		y1: number,
		x2: number,
		y2: number
	) => {
		if (!ctx || !x1 || !y1 || !x2 || !y2) return;
		ctx.beginPath();
		ctx.strokeStyle = 'red';
		ctx.lineWidth = 2;
		ctx.moveTo(x1, y1);
		ctx.lineTo(x2, y2);
		ctx.stroke();
		ctx.closePath();
	};

	const drawRect = (
		ctx: CanvasRenderingContext2D,
		x1: number,
		y1: number,
		x2: number,
		y2: number
	) => {
		if (!ctx || !x1 || !y1 || !x2 || !y2) return;
		ctx.beginPath();
		ctx.strokeStyle = 'red';
		ctx.lineWidth = 2;
		ctx.moveTo(x1, y1);
		ctx.strokeRect(x1, y1, x2 - x1, y2 - y1);
		ctx.stroke();
		ctx.closePath();
	};

	onMount(() => {
		const context = canvas.getContext('2d', { willReadFrequently: true });
		if (context) {
			ctx = context;
		}
	});
</script>

<h1>Canvas</h1>
<button class="rounded-md bg-gray-500 p-2" on:click={clearCanvas}>ClearCanvas</button>
<canvas
	class={` ${drawing ? 'cursor-crosshair' : 'cursor-default'}`}
	on:mousemove={onMouseMove}
	on:mouseup={onMouseUp}
	on:mousedown={onMouseDown}
	bind:this={canvas}
	id="canvas"
	height="150"
	width="300"
>
	>
	<h2>mahesh car</h2>
</canvas>
