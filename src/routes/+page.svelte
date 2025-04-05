<script lang="ts">
	let canvas: HTMLCanvasElement;
	let drawing = false;
	let x = 0;
	let y = 0;
	const clearCanvas = () => {
		let ctx = canvas.getContext('2d');
		console.log({
			canvasContext: ctx
		});
		if (ctx) {
			ctx.reset();
		}
	};

	const onMouseDown = (e: MouseEvent) => {
		// When user clicks before releasing
		x = e.offsetX;
		y = e.offsetY;
		drawing = true;
	};

	const onMouseUp = (e: MouseEvent) => {
		if (drawing) {
			let ctx = canvas.getContext('2d');
			if (!ctx) return;
			drawLine(ctx, x, y, e.offsetX, e.offsetY);
			x = 0;
			y = 0;
			drawing = false;
		}
		x = e.offsetX;
		y = e.offsetY;
		drawing = false;
	};

	const onMouseMove = (e: MouseEvent) => {
		if (!drawing) return;
		let ctx = canvas.getContext('2d');
		if (!ctx) return;
		drawLine(ctx, x, y, e.offsetX, e.offsetY);
		x = e.offsetX;
		y = e.offsetY;
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
</script>

<h1>Canvas</h1>
<button class="rounded-md bg-gray-500 p-2" on:click={clearCanvas}>ClearCanvas</button>
<canvas
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
