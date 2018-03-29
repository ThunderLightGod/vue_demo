<template>
  <div>
      <canvas ref="snowflake" id="snowflake"></canvas>
  </div>
</template>
<script>
export default {
  data () {
    return {
    }
  },
  props: {
    config: {
      type: Object
    }
  },
  methods: {
  }
}
</script>
<style scoped>
#snowflake{
    position:absolute;
    left:0;
    top:0;
    z-index: 999999;
}
</style>
