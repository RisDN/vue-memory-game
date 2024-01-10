<script setup>

import { ref } from "vue";

const cells = ref([]);

let canClick = false;
let guesses = 0;

async function nextRound() {

	canClick = false;

	let randomCellId = getRandomCellId();

	cells.value.push(randomCellId)

	for(let i = 0; i < cells.value.length; i++) {
		let index = cells.value[i];
		document.querySelector(`#cell-${index}`).style.backgroundColor = "yellow"
		await timer(500);
		document.querySelector(`#cell-${index}`).style.backgroundColor = "grey"
	}

	canClick = true
}


async function clickHandler(cellId) {

	if(!canClick) {
		return;
	}

	if(cells.value[guesses] != cellId) {
		alert("Vesztettél! Elért pontszám:" + cells.value.length)
		return;
	}

	guesses++;
	document.querySelector(`#cell-${cellId}`).style.backgroundColor = "green";
	await timer(100)
	document.querySelector(`#cell-${cellId}`).style.backgroundColor = "grey";

	if(guesses != cells.value.length) {
		return;
	}

	guesses = 0;
	canClick = false;
	await timer(500)
	nextRound()

}



function timer(ms) { return new Promise(res => setTimeout(res, ms)); }

function getRandomCellId() {
  return Math.floor(Math.random() * 9) + 1
}

</script> 

<template>

<div class="controls">
	<h2>Kör: {{ cells.length }}</h2>
</div>
<div class="grid">

	<div class="row">
    	<div v-on:click="clickHandler(1)" id="cell-1" class="cell"></div>
		<div v-on:click="clickHandler(2)" id="cell-2" class="cell"></div>
    	<div v-on:click="clickHandler(3)" id="cell-3" class="cell"></div>
	</div>
	<div class="row">
		<div v-on:click="clickHandler(4)" id="cell-4" class="cell"></div>
		<div v-on:click="clickHandler(5)" id="cell-5" class="cell"></div>
		<div v-on:click="clickHandler(6)" id="cell-6" class="cell"></div>
	</div>
	<div class="row">
		<div v-on:click="clickHandler(7)" id="cell-7" class="cell"></div>
		<div v-on:click="clickHandler(8)" id="cell-8" class="cell"></div>
		<div v-on:click="clickHandler(9)" id="cell-9" class="cell"></div>
	</div>
</div>

<div class="controls">
	<button v-on:click="nextRound">Start</button>
</div>


</template>

<style scoped>
.grid {
  display: table;
  border-spacing: 5px;
  margin: 0 auto;
}
.row {
  display: table-row
}
.cell {
  width: 150px;
  height: 150px;
  background: grey;
  display: table-cell;
  cursor: pointer;
}

.controls {
	text-align: center;
	margin: 5%;
}
.controls button {
	width: 75px;
	height: 35px;
	cursor: pointer;
}
</style>
