<template>
    <SlideshowContainer id="sl1">
        <h1>Slide #1</h1>
        <LoremIpsum />
    </SlideshowContainer>
    <SlideshowContainer id="sl2">
        <h1>Slide #2</h1>
        <LoremIpsum />
    </SlideshowContainer>
    <ParallaxContainer>
        <LoremIpsum :t="10" />
    </ParallaxContainer>

    <ScrollLinked tag="header" v-model:absolute="absolute" :class="headerClass" @mouseover="() => active = true" @mouseout="() => active = false">
        <h1>Menu</h1>
    </ScrollLinked>

</template>

<script lang="ts">
import LoremIpsum from './LoremIpsum.vue';
import ParallaxContainer from '../../../src/ParallaxContainer.vue';
import SlideshowContainer from '../../../src/SlideshowContainer.vue';
import ScrollLinked from '../../../src/ScrollLinked.vue';

export default {
    data() {
        return{
            absolute: {x:0, y: 0},
            active: false
        };
    },
    computed: {
        headerClass() {
            return this.absolute.y >= -60 || this.active ? "active" : "";
        }
    },
    components: { LoremIpsum, ParallaxContainer, SlideshowContainer, ScrollLinked }
}
</script>

<style>

div.scroll-linked {
    height: 100vh;
    width: 100vw;
    position: absolute;
}

h1, h2 {
    font-family: Arial, Helvetica, sans-serif;
    font-weight: bold;
}

#sl1 {
    top:0;
}
#sl1 .content{
    background-image: url('shepherd.jpg');
    color: #FFF;
}
#sl2 {
    top:100vh;
}
#sl2 .content{
    background-image: url('tree-frog.jpg');
    color: #FFF;
}

.slideshow .content {
    background-size: cover;
}

.slideshow p{
    margin: 16px 32px; 
}

.slideshow h1, .slideshow h2 {
    margin: 16px 48px;
}
    
.parallax {
    background: url('clown-fish.jpg');
    background-size: 200%;
    top: 200vh; 
}

.parallax .content {
    position: relative;
    transition: all 1s ease-out;
    top: 400px;
    margin: 30px 80px;
    color: #FFF;
    font-weight: bold; 
    opacity: 0;
    width: 40vw;
}

.parallax .content.in-view {
    top: 50px;
    opacity: 1;
}


header {
    background: #5A0;
    Color: #FFF;
    position: fixed;
    width: 100%;
    transition: top .8s ease-out;
    box-shadow: 0 -10px 20px 5px black;
    top: -50px;
    height: 60px;
}

header.active {
    top: 0px;
}
</style>