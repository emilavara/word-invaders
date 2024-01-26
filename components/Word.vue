<template>
    <span ref="hejsan" :style="{left: word.ypos + '%'}" class="word">
        {{word.word}}
    </span>
</template>

<script>
import { gsap } from 'gsap'

    export default {
        props: [
            'word'
        ],
        mounted() {

        }
    }
</script>

<style lang="scss">
.word {
    color: white;
    position: absolute;
    top: 0%;
    width: fit-content;
    height: 0;
    animation: collision 30s linear;
    // transition: top 30s;
    /* top: 100%; */

    @keyframes collision {
        0% {
            top: -2%;
            color: white;
        }
        49% {
            top: 49%;
            color: white;
        }
        50% {
            top: 50%;
            color: var(--yellow);
        }
        74% {
            top: 74%;
            color: var(--yellow);
        }
        75% {
            top: 75%;
            color: var(--red);
        }
        100% {
            top: 100%;
            color: var(--red);
        }
    }
}
</style>