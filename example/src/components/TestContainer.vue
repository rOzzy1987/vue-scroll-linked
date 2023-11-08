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

</template>

<script lang="ts">
import LoremIpsum from './LoremIpsum.vue';
import ParallaxContainer from '../../../src/ParallaxContainer.vue';
import SlideshowContainer from '../../../src/SlideshowContainer.vue';

export default {
    data() {
        return{};
    },
    components: { LoremIpsum, ParallaxContainer, SlideshowContainer }
}
</script>

<style>

.scroll-linked {
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
</style>