<template>
  <div>
    <h1>Heatmaps</h1>
    <img id="heatmap" v-bind:src="imageSrc[currentImg]" />
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  mounted () {
    this.imgReload(this)
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
        `http://${window.location.hostname}:3000/floor1/latest.png?`,
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
    displayRotation: (vm) => {
      vm.currentImg = (vm.currentImg + 1) % 3
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
