<template>
    <div class="wi-game-container">
        <div class="debug">
            <pre>
                {{spawned}}
            </pre>
        </div>
        <div class="canvas">
            <Word :word="word" :data-index="index" v-for="word, index in spawned" :key="index"/>
        </div>
        <div class="hud">
            <div class="heart-container">
                <svg v-for="heart in hearts" width="27" height="25" viewBox="0 0 27 25" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M13.5 25L11.5425 23.2016C4.59 16.8392 0 12.6294 0 7.49319C0 3.28338 3.267 0 7.425 0C9.774 0 12.0285 1.10354 13.5 2.83379C14.9715 1.10354 17.226 0 19.575 0C23.733 0 27 3.28338 27 7.49319C27 12.6294 22.41 16.8392 15.4575 23.2016L13.5 25Z" fill="white"/>
                </svg>
            </div>

            <div class="input">
                <input 
                @keydown.space.prevent 
                @input="completeWord"
                @blur="keepInputFocused" 
                ref="userInput" 
                v-model="input" 
                placeholder="type here..."
                type="text"/>
            </div>

            <div class="score">
                <span>{{score}}</span>
            </div>
        </div>
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
            hearts: 3,
            input: '',
            score: 0,
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
       completeWord() {
            var audio = new Audio('/audio/complete.ogg');

            this.spawned.forEach((item, index) => {
                if (item.word === this.input) {
                    var spawnedWords = document.querySelectorAll('.word');
                    
                    spawnedWords.forEach((word) => {
                        if(word.dataset.index == index) {
                            word.style.visibility = 'hidden';
                        }
                    })
                    
                    this.input = ''
                    this.score += 1
                    // audio.play();
                }
            });
        },
        endGame() {
            clearInterval(this.running);

            var spawnedWords = document.querySelectorAll('.word');
                    
            spawnedWords.forEach((word) => {
                word.style.animationPlayState = 'paused'
            })
        },
        keepInputFocused() {
            this.$refs.userInput.focus();
        }
    },
    watch: {
        hearts() {
            if (this.hearts == 0) {
                this.endGame();
            }
        }
    },
    mounted() {
        this.$refs.userInput.focus();
        this.startGame()

        window.addEventListener('animationend', (e) => {
            console.log(e.srcElement.style)
            if(e.srcElement.style.visibility == 'hidden') {
                console.log('WOOOW THAT WAS HIDDEN')
                e.srcElement.remove();
            } else {
                e.srcElement.remove();
                this.hearts -= 1;
            }
        })
    }
}
</script>