<svelte:head>
	<title>Bingo</title>
	<meta name="description" content="Bingo description" />
</svelte:head>

<script lang="ts">
	import { onMount } from 'svelte';

	onMount(() => {
		console.log('Bingo page mounted');
	});

	const shuffle = (arr: number[]) => {
		let currentIndex = arr.length, randomIndex;

		while (currentIndex != 0) {
			randomIndex = Math.floor(Math.random() * currentIndex);
			currentIndex--;

			[arr[currentIndex], arr[randomIndex]] = [arr[randomIndex], arr[currentIndex]];
		}

		return arr;
	}

	const getRandom = (min: number, max: number): number[] => {
		// const arr = Math.floor(Math.random() * (max - min + 1)) + min;
		const arr = [];
		for (let i = 0; i < 15; i++) {
			arr.push(i + min);
		}

		const shuffled = shuffle(arr);
		const sets = shuffled.slice(0, 5);

		return sets;

	}

	let setB = getRandom(1, 15);
	let setI = getRandom(16, 30);
	let setN = getRandom(31, 45);
	let setG = getRandom(46, 60);
	let setO = getRandom(61, 75);

	const saveCard = () => {
		const card = {
			B: setB,
			I: setI,
			N: setN,
			G: setG,
			O: setO
		};

		localStorage.setItem('bingo-card', JSON.stringify(card));
	}
	
	const loadCard = () => {
		const card = localStorage.getItem('bingo-card');
		if (card) {
			const parsed = JSON.parse(card);
			setB = parsed.B;
			setI = parsed.I;
			setN = parsed.N;
			setG = parsed.G;
			setO = parsed.O;
		}
	}

	const clearCard = () => {
		setB = getRandom(1, 15);
		setI = getRandom(16, 30);
		setN = getRandom(31, 45);
		setG = getRandom(46, 60);
		setO = getRandom(61, 75);

		removeAllOn();
	}

	const removeAllOn = () => {
		const cells = document.querySelectorAll('.cell');
		cells.forEach(cell => {
			cell.classList.remove('on');
		});
	}

	const toggleOn = (e: MouseEvent) => {
		const target = e.currentTarget as HTMLButtonElement;
		target.classList.toggle('on');
	}
</script>

<div class="text-column">
	<h1>My Bingo Card</h1>

	<div class="main-card">
		<div class="row row-top">
			<div class="cell {true ? '' : 'on'}">B</div>
			<div class="cell">I</div>
			<div class="cell">N</div>
			<div class="cell">G</div>
			<div class="cell">O</div>
		</div>

		<div class="row">

			<div class="col col-b">
				{#each setB as number}
					<button class="cell" on:click={(event) => toggleOn(event)}>
						{number}
					</button>
				{/each}
			</div>

			<div class="col col-c">
				{#each setI as number}
					<button class="cell" on:click={(event) => toggleOn(event)}>
						{number}
					</button>
				{/each}
			</div>
			<div class="col col-n">
				{#each setN as number}
				<button class="cell" on:click={(event) => toggleOn(event)}>
					{number}
				</button>
				{/each}
			</div>
			<div class="col col-g">
				{#each setG as number}
				<button class="cell" on:click={(event) => toggleOn(event)}>
					{number}
				</button>
				{/each}
			</div>
			<div class="col col-o">
				{#each setO as number}
				<button class="cell" on:click={(event) => toggleOn(event)}>
					{number}
				</button>
				{/each}
			</div>
		</div>
	</div>

	<div class="row wrap actions">
		<button on:click={clearCard}>Clear My Card</button>
	</div>
	<div class="row wrap actions">
		<button on:click={loadCard}>Load My Card</button>
		<button on:click={saveCard}>Save New Card</button>
	</div>
</div>

<style>
.main-card {
	border: 1px solid black;
}
.row {
	display: flex;
	flex-direction: row;
}
.col {
	display: flex;
	flex-direction: column;
}
.wrap {
	flex-flow: row wrap;
}
.cell {
	width: 50px;
	height: 50px;
	display: flex;
	align-items: center;
	justify-content: center;
	border: 1px solid black;
	padding: 0;
	box-sizing: content-box;
	background-color: #f2f2f2;
}
.cell.on {
	background-color: lightblue;
}
.actions {
	margin: 16px 0;
	justify-content: space-between;
}
</style>