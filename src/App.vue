<template>
	<div id="app">
		<progress-bar :solvedIssuesPercentage="solvedIssuesPercentage"/>
		<h1>Tarefas</h1>
		<issue-creator @issueAdded="addIssue" />
		<div v-if="hasIssue" class="issueContainer">
			<issue 
				v-for="(issue, i) in issues" :key="i" 
				:issue="issue" 
				@issueDeleted="deleteIssue" @issueStateChanged="updateSolvedIssues"/>
		</div>
		<p v-else>There are no issues</p>
	</div>
</template>

<script>
import ProgressBar from '@/components/ProgressBar';
import IssueCreator from '@/components/IssueCreator';
import Issue from '@/components/Issue'

export default {
	components: {
		ProgressBar, IssueCreator, Issue
	},

	data() {
		return {
			issues: {},
			solvedIssues: {}
		}
	},

	methods: {
		addIssue(issue) {
			const index = Object.entries(this.issues).length;
			this.$set(issue, 'index', index);
			this.$set(this.issues, index, issue);
		},

		deleteIssue(index) {
			this.$delete(this.issues, index);
			this.$delete(this.solvedIssues, index);
		},

		updateSolvedIssues(issue) {
			if (issue.state) {
				this.$set(this.solvedIssues, issue.index, issue);
				return;
			}

			const solvedIssue = this.solvedIssues[issue.index];
			if (!solvedIssue) {
				return;
			}

			this.$delete(this.solvedIssues, issue.index);
		}
	},

	computed: {
		hasIssue() {
			return this.issues.length != 0;
		},
		
		solvedIssuesPercentage() {
			const solvedCount = Object.entries(this.solvedIssues).length;
			if (!solvedCount) {
				return 0;
			}

			const totalCount = Object.entries(this.issues).length;
			return Number(solvedCount * 100 / totalCount).toFixed(2);
		}
	}

}
</script>

<style>
	.issueContainer {
		display: flex;
		width: 80%;
		flex-wrap: wrap;
    justify-content: flex-start;
    align-items: baseline;
    padding: 5%;
		flex-grow: 1;
	}

	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}

	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>
