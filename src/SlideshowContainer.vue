<template>
    <ScrollLinked 
        v-model:phase="phase"
        v-model:size="size"
        class="slideshow">
        <div class="content" :style="style">
            <slot></slot>
        </div>
    </ScrollLinked>
</template>

<script lang="ts">
import ScrollLinked from './ScrollLinked.vue';

export default {
    data() {
        return {
            phase: { x: 0, y: 0 },
            size: { x: 0, y: 0 }
        };
    },
    computed: {
        style() {
            return `top: ${((this.phase.y - .5) * 2) * this.size.y}px`;
        },
    },
    components: { ScrollLinked }
}
</script>

<style>
.slideshow {
    overflow: hidden;
}
.slideshow .content {
    position: absolute;
    width: 100%;
    height: 100%;
}
</style>