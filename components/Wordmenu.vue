<template>
    <span :style="{left: word.ypos + '%'}" class="word-menu">
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
.word-menu {
    color: white;
    opacity: 0.25;
    pointer-events: none;
    position: absolute;
    top: 0%;
    width: fit-content;
    height: 0;
    animation: collision-menu 15s linear forwards;
    // transition: top 30s;
    /* top: 100%; */

    @keyframes collision-menu {
        0% {
            top: -2%;
        }
        49% {
            top: 49%;
        }
        50% {
            top: 50%;
        }
        74% {
            top: 74%;
        }
        75% {
            top: 75%;
        }
        100% {
            top: 100%;
        }
    }
}
</style>