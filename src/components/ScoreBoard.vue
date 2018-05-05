<template>
	<table>
		<thead>
			<tr>
				<th>#</th>
				<th>Name</th>
				<th>Time</th>
				<th>At</th>
			</tr>
		</thead>
		<tr v-for="(score, index) in scores" :key="index">
			<td>{{ index + 1 }}</td>
			<td>{{ score.name }}</td>
			<td>{{ score.time.toFixed(3) }}</td>
			<td class="lighter" :title="score.created ? $moment(score.created).format('YYYY-MM-DD HH:mm:ss.ms') : null">{{ score.created ? $moment(score.created).fromNow() : "-" }}</td>
		</tr>
	</table>
</template>

<script>
export default {
  data() {
    return {
      loadingScores: true,
      scores: []
    };
  },
  props: {
    type: String
  },
  mounted() {
    this.fetchScoreboard();
  },
  methods: {
    fetchScoreboard() {
      this.$http
        .get(this.url)
        .then(res => {
          this.scores = res.data;
          this.loadingScores = false;
        })
        .catch(console.error);
    }
  },
  watch: {
    url() {
      this.fetchScoreboard();
    }
  },
  computed: {
    url() {
      switch (this.type) {
        case "last":
          return "/scores/last";
        case "daily":
          return "/scores/daily";
        case "weekly":
          return "/scores/weekly";
        default:
          return "/scores";
      }
    }
  }
};
</script>

<style scoped>
table {
  text-align: left;
}
th:first-child,
td:first-child {
  text-align: right;
  font-weight: bold;
}
td,
th {
  padding: 0.5em;
}
thead tr {
  border-bottom: 2px #eee solid;
  color: rgb(100, 100, 100);
}
table tr:not(:last-child) {
  border-bottom: 1px #eee solid;
  padding: 5px;
}
.lighter {
  color: rgb(120, 120, 120);
}
</style>