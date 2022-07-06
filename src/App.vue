<script setup>
import { ref, computed, onMounted } from 'vue'

const wins = ref(0)
const draws = ref(0)
const losses = ref(0)

const choice = ref(null)
const computerChoice = ref(null)
const verdict = ref(null)

const outcomes = {
	'rock': {
		'rock': 'draw',
		'paper': 'loss',
		'scissors': 'win'
	},
	'paper': {
		'rock': 'win',
		'paper': 'draw',
		'scissors': 'loss'
	},
	'scissors': {
		'rock': 'loss',
		'paper': 'win',
		'scissors': 'draw'
	}
}

const play = (c) => {
	choice.value = c
	
	const choices = ['rock', 'paper', 'scissors']
	const random = Math.floor(Math.random() * choices.length)
	computerChoice.value = choices[random]

	const outcome = outcomes[choice.value][computerChoice.value]
	
	if (outcome === 'win') {
		wins.value++
		verdict.value = 'You win!'
	} else if (outcome === 'loss') {
		losses.value++
		verdict.value = 'You Lose!'
	} else {
		draws.value++
		verdict.value = 'It\'s a draw'
	}

	SaveGame()
}

const winPercentage = computed(() => {
	const total = wins.value + draws.value + losses.value
	return total ? (wins.value / total) * 100 : 0
})

const SaveGame = () => {
	localStorage.setItem('wins', wins.value)
	localStorage.setItem('draws', draws.value)
	localStorage.setItem('losses', losses.value)
}

const LoadGame = () => {
	wins.value = parseInt(localStorage.getItem('wins'))
	draws.value = parseInt(localStorage.getItem('draws'))
	losses.value = parseInt(localStorage.getItem('losses'))
}

const ResetRound = () => {
	choice.value = null
	computerChoice.value = null
	verdict.value = null
}

onMounted(() => {
	LoadGame()
})
</script>

<template>
	<div class="bg-gray-700 text-white min-h-screen flex flex-col">
		<header class="container mx-auto px-6 py-4">
			<h1 class="text-4xl font-bold text-center">Rock, Paper, Scissors!</h1>
		</header>

		<main class="container mx-auto px-6 py-4 flex-1">
			<div v-if="choice === null" class="flex items-center justify-center -mx-6">
				
				<button @click="play('rock')" class="mx-6">
					<img src="./assets/RockIcon.svg" alt="Rock" />
				</button>

				<button @click="play('paper')" class="mx-6">
					<img src="./assets/PaperIcon.svg" alt="Paper" />
				</button>

				<button @click="play('scissors')" class="mx-6">
					<img src="./assets/ScissorsIcon.svg" alt="Scissors" />
				</button>

			</div>

			<div v-else class="verdict">
				<h2>{{ choice }}</h2>
				<h2>{{ computerChoice }}</h2>
				<h2>{{ verdict }}</h2>

				<button @click="ResetRound">Reset</button>
			</div>

			<div>{{ wins }} : {{ draws }} : {{ losses }}</div>

			<div>Win rate: {{ Math.round(winPercentage) }}%</div>
		</main>

		<footer class="container mx-auto px-6 py-4">
			<a href="https://www.vecteezy.com/free-vector/rock-paper-scissors-game">Rock Paper Scissors Game Vectors by Vecteezy</a>
		</footer>
	</div>
</template>

<style></style>
