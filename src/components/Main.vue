<template>
  <div>
    <h1>Heatmaps</h1>
    <canvas id="heatmap" width="897px" height="644px" />
    <img id="img" width="100%" v-bind:src="imageSrc[currentImg]" style="display:none;" v-on:load="imgRender()"/>
  </div>
</template>

<script>
export default {
  mounted () {
    this.ctx = document.getElementById('heatmap').getContext('2d')
    this.imgReload(this)
    this.imgRender()
    setInterval(this.imgReload, 60 * 1000, this)
    setInterval(this.displayRotation, 10 * 1000, this)
  },
  destroyed () {
    clearInterval(this.imgReload)
    clearInterval(this.displayRotation)
  },
  data () {
    return {
      imageSrc: [
        // `http://${window.location.hostname}:3000/floor1/latest.png?`,
        `http://${window.location.hostname}:3000/floor2/latest.png?`
      ],
      currentImg: 0
    }
  },
  methods: {
    imgReload: (vm) => {
      vm.imageSrc = vm.imageSrc.map(
        str => `${str.split('?')[0]}?${Date.now().toString()}`
      )
    },
    imgRender: () => {
      let ctx = document.getElementById('heatmap').getContext('2d')
      ctx.drawImage(document.getElementById('img'), 150, 150, 780, 560, 0, 0, 897, 644)
    },
    displayRotation: (vm) => {
      vm.currentImg = (vm.currentImg + 1) % vm.imageSrc.length
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#heatmap {
  width: 100%;
}
</style>
