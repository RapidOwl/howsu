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
		const style = {
			transform: `scale(${scale})`,
			transformOrigin: 'top left',
			width: categoryForm.clientWidth + 'px', // use original width of DOM element to avoid part of the image being cropped out
			height: categoryForm.clientHeight + 'px', // use original height of DOM element
		};

		domtoimage
			.toBlob(categoryForm, {
				// These options scale the image to avoid blurry output.
				width: categoryForm.clientWidth * scale,
				height: categoryForm.clientHeight * scale,
				style: style,
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
