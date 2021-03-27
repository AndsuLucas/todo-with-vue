<template>
  <div class="issueCard" :class="stateClass" @click="changeIssueState">
    <p>{{issue.text}}</p>
    <span @click="deleteIssue">&#10006;</span>
  </div>
</template>
<script>
export default {
  props: {
    issue: {
      type: Object,
      required: true
    }
  },

  methods: {
    changeIssueState(evt) {
      evt.stopPropagation();
      const actualState = this.issue.state; 
      const message = actualState ? 'Unsolve this issue ?' : 'Solve this issue ?';

      const answer = confirm(message);
      if (!answer) {
        alert('Process canceled');
        return;
      }

      if (actualState) {
        this.$set(this.issue, 'state', 0);
        return;
      }

      this.$set(this.issue, 'state', 1);
    },
    
    deleteIssue(evt) {
      evt.stopPropagation();
      const answer = confirm('Delete this issue?');
      if (!answer) {
        alert('Process canceled');
        return;
      }

      this.$emit('issueDeleted', this.issue.index);
    }
  },
  
  updated() {
    this.$emit('issueStateChanged', this.issue);
  },

  computed: {
    stateClass() {
      if (!this.issue.state) {
        return 'unsolvedIssue';
      }

      return 'solvedIssue';
    }
  }
}
</script>
<style scoped>
  .issueCard > p {
    max-width: 100%;
  }
  
  .issueCard > span {
    position: absolute;
    right: 0;
    top: 0;
    z-index: 1000;
  }

  .issueCard {
    position: relative;
    border: 0.5px solid white;
    padding: 1%;
    margin: 1%;
    max-width: 40%;
    word-wrap: break-word;
    border-radius: 5%;
    cursor: pointer;
    display: flex;
  }

  .unsolvedIssue {
    background: #ed213a; /* fallback for old browsers */
    background: -webkit-linear-gradient(to right, #ed213a, #93291e); /* Chrome 10-25, Safari 5.1-6 */
    background: linear-gradient(to right, #ed213a, #93291e); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  }

  .solvedIssue > p {
    text-decoration: line-through;
  }

  .solvedIssue {
    background: #76b852;  /* fallback for old browsers */
    background: -webkit-linear-gradient(to right, #8DC26F, #76b852);  /* Chrome 10-25, Safari 5.1-6 */
    background: linear-gradient(to right, #8DC26F, #76b852); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  }
</style>