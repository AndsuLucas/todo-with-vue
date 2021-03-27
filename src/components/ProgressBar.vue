<template>
  <div class="progressBarContainer">
    <p>{{ solvedIssuesPercentage }}%</p>
    <progress :value="solvedIssuesPercentage" max="100"></progress>
  </div>
</template>
<script>
export default {  
  props: {
    solvedIssuesPercentage: {
      type: Number,
      default: 0
    }
  }
}
</script>
<style scoped>
  progress {
    min-width: 100%;
    width: 30px;
  }

  .progressBarContainer p{
    text-align: center;
  }

  .progressBarContainer {
    min-width: 80%;
  }
</style>