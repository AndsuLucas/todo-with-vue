<template>
  <div>
    <input type="text" v-model="issue.text" @keyup.enter="addIssue">
    <button @click="addIssue">Add</button>  
  </div>
</template>
<script>
export default {
  data() {
    return {
      issue: {
        text: '',
        state: 0
      }
    }
  },

  methods: {
    addIssue() {
      if (!this.issue.text.trim()) {
        alert('Empty issue is not allowed');
        return;
      }

      this.$emit('issueAdded', Object.assign({}, this.issue));
      this.issue.text = '';
    }
  }
}
</script>
<style scoped>
  div {
    min-width: 80%;
    display: flex;
  }

  div > input {
    flex-grow: 2;
    height: 30px;
    position: static;
  }

</style>