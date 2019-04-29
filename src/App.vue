<template>
  <div class="main" :style="{height: mainHeight + 'px'}">
    <div class="picture"></div>
    <div v-for="i in 3" :key="i" :class="`content content-${i}`" ref="columns">
        <div ref="texts">{{text}}</div>
        <div :style="{width: '1px', height: `${spacerHeight}px`}"></div>
    </div>
  </div>
</template>

<script>
export default {
    data: () => ({
        text: `Lorem ipsum dolor sit, amet consectetur adipisicing elit. Temporibus fuga corporis magni cumque, earum vel necessitatibus beatae excepturi ex unde repellendus dolores a nemo deserunt quas vero quis? Omnis, eaque!
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Reprehenderit ratione obcaecati reiciendis eaque? Totam, vel repudiandae? Minima unde id cum. Nemo minus voluptatibus impedit atque quidem neque iusto, blanditiis est. 
            Lorem ipsum dolor sit, amet consectetur adipisicing elit. Temporibus fuga corporis magni cumque, earum vel necessitatibus beatae excepturi ex unde repellendus dolores a nemo deserunt quas vero quis? Omnis, eaque!
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Reprehenderit ratione obcaecati reiciendis eaque? Totam, vel repudiandae? Minima unde id cum. Nemo minus voluptatibus impedit atque quidem neque iusto, blanditiis est.`,
        pictureHeight: 200,
        lineHeight: 20,
        scrollHeight: 0
    }),
    computed: {
        spacerHeight() {
            return this.mainHeight - this.pictureHeight;    
        },
        mainHeight() {
            return Math.floor((this.scrollHeight + 2*this.pictureHeight) / 3 / this.lineHeight + 1) * this.lineHeight;
        }
    },
    methods: {
        updateScrollHeight() {
            this.scrollHeight = this.$refs.texts[0].scrollHeight;
            requestAnimationFrame(() => this.$refs.columns.reduce((acc, col) => {
                col.scrollTop = acc;
                return acc + col.getBoundingClientRect().height;
            }, 0))
        }
    },
    mounted() {
        this.updateScrollHeight();
        window.addEventListener('resize', this.updateScrollHeight);
    }
}
</script>


<style>
.main {
	display: grid;
    grid-column-gap: 1em;
    grid-template-columns: repeat(3, minmax(200px, 1fr));
    grid-template-rows: 200px auto;
    background: #333;
    color: #ddd;
    position: relative;
    font-size: 16px;
    line-height: 20px;
    padding: 1em;
    box-sizing: content-box;
}
.picture {
    grid-column: 2 / span 2;
}
.content {
    overflow: hidden;
}
.content-1 {
    grid-row: 1 / span 2;
}

</style>
