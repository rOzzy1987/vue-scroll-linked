<template>
    <ScrollLinked 
        v-model:phase="phase"
        v-model:shown-already="shownAlready"
        :style="style" 
        class="parallax">
        <div :class="cssClass">
            <slot></slot>
        </div>
    </ScrollLinked>
</template>

<script lang="ts">
import ScrollLinked from './ScrollLinked.vue';

export default {
    data() {
        return {
            shownAlready: { x: 0, y: 0},
            phase: { x: 0, y: 0},
        };
    },
    props: {
        backgroundMovementY: {type: Number, default: 800},
        contentRatioThreshold: {type: Number, default: .5},

    },
    computed: {
        style() {
            return `background-position-y: ${-this.backgroundMovementY + this.phase.y * this.backgroundMovementY}px`;
        },
        cssClass(){
            return "content " + (this.shownAlready.y > this.contentRatioThreshold ? "in-view" : "");
        }
    },
    components: { ScrollLinked }
}
</script>