<script>
	import domtoimage from 'dom-to-image';

	let categories = [
		{ label: 'Body', score: 0 },
		{ label: 'Mind', score: 0 },
		{ label: 'Work', score: 0 },
		{ label: 'Love', score: 0 },
		{ label: 'Pandemic', score: 0 },
	];

	let categoryForm;

	function updateScore(categoryIndex, score) {
		categories[categoryIndex].score = score;
	}

	function saveImage() {
		const scale = 2;

		domtoimage
			.toBlob(categoryForm, {
				// These options scale the image to avoid blurry output.
				height: categoryForm.offsetHeight * scale,
				style: {
					transform: `scale(${scale}) translate(${
						categoryForm.offsetWidth / 2 / scale
					}px, ${categoryForm.offsetHeight / 2 / scale}px)`,
				},
				width: categoryForm.offsetWidth * scale,
			})
			.then(function (blob) {
				window.saveAs(blob, 'howsu.png');
			});
	}
</script>

<main>
	<h1>Hello!</h1>
	<p>Fill in this interesting form</p>

	<section bind:this={categoryForm} class="category-form">
		<ul>
			{#each categories as category, categoryIndex}
				<li class="category">
					<div class="category-label">{category.label}</div>
					{#each Array(5) as _, i}
						<div
							class="score-box {category.score >= i + 1 ? 'selected' : ''}"
							on:click={() => updateScore(categoryIndex, i + 1)}
						/>
					{/each}
				</li>
			{/each}
		</ul>
	</section>
	<button on:click={() => saveImage()}>Save as Image</button>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		margin: 0 auto;
	}

	ul {
		margin: 0;
		padding: 0;
	}

	.category-form {
		border: 4px solid black;
		text-align: center;
		padding: 24px;
		background-color: white;
	}

	.category {
		display: flex;
		justify-content: center;
	}

	.category-label {
		width: 90px;
		text-align: left;
		line-height: 2rem;
	}

	.score-box {
		border: 2px solid black;
		height: 20px;
		width: 20px;
		margin: 4px;
	}

	.score-box.selected {
		background-color: red;
	}
</style>
