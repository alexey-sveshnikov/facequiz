<template>
  <div class="progressbar">
    <div class="filled" :style="filledStyle">
      &nbsp;
    </div>
  </div>

</template>

<script>
  export default {
    name: "ProgressBar",
    props: ['progress'],
    // data: function() {
    //   return {
    //     progress: 20,
    //   }
    // },
    computed: {
      filledStyle: function() {
        console.log(`Progress: <<${this.progress}>>`);
        return {
          'width': `${this.progress}` + '%',
        }
      },
    }
  }
</script>

<style scoped lang="scss">
  .progressbar {
    background: #cceccc;
    text-align: left;
    display: flex;
    margin: auto;
  }
  .filled {
    background: #42b983;
    display: inline-block;
    left: 0;
  }
</style>
