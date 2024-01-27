<template>
	<div class="wi-menu-container">
		<div class="wi-title">word invaders</div>
		<div class="wi-menu">
			<span @click="$router.push('/game')">start</span>
			<span>how to play</span>
			<span>options</span>
		</div>
    	<Wordmenu :word="word" v-for="word, index in spawned" :key="index"/>

	</div>
</template>

<script>
import wordlist from '~/components/wordlist.json';

export default {
	data() {
		return {
			words: wordlist,
            spawned: [
            ],
            running: ''

		}
	},
	methods: {
		startGame() {
            this.running = setInterval(this.spawnWord, 1000);
        },
		spawnWord() {
            var r = Math.floor(Math.random()*this.words.length);
            var rYPos = Math.floor(Math.random() * 90);

            var spawnedObj = {}
            spawnedObj.word = this.words[r]
            spawnedObj.ypos = rYPos

            this.spawned.push(spawnedObj)
        },
	},
	mounted() {
		this.startGame();
	}
}
</script>