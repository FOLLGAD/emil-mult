<template>
  <div class="spread">
		<div class="container">
			<header-text large>Mult</header-text>
			<div class="container">
				<h2>Scoreboard</h2>
				<div class="scoreboard-tabs container">
					<a v-for="tab in ['last', 'daily', 'weekly', 'all-time']" :key="tab" href="javascript:void(0)" class="link tab" @click="type = tab" :class="{ active: type == tab }">
						{{ tab | sentence }}
					</a>
				</div>
				<div class="mb-2">
					<score-board class="mh-auto scoreboard" :type="type" ref="scoreboard" />
				</div>
			</div>
			<div class="col" v-if="registeredTime > 0 && !posted">
				<div>
					<h2>Your result</h2>
					<p>Your time was {{ registeredTime }} seconds</p>
				</div>
				<input v-model="name" class="number-input mb-2" type="text" autofocus="true" placeholder="Enter a name" />
				<styled-button :onclick="postResult">Post</styled-button>
			</div>
			<div class="mb-2">
				<styled-button :onclick="() => $router.push('/play')">
					Play
				</styled-button>
			</div>
		</div>
		<div class="grey"><span>Made by Empinini ;)</span></div>
  </div>
</template>

<script>
import ScoreBoard from "@/components/ScoreBoard";
import StyledButton from "@/components/Button";
import HeaderText from "@/components/Header";

export default {
  components: {
    ScoreBoard,
    HeaderText,
    StyledButton
  },
  props: {
    registeredTime: Number,
    max: Number,
    min: Number
  },
  methods: {
    postResult() {
      if (this.posting || this.posted) return;

      this.posting = true;

      this.$http
        .post("/scores", {
          time: this.registeredTime,
          max: this.max,
          min: this.min,
          name: this.name
        })
        .then(d => {
          this.$refs.scoreboard.fetchScoreboard();
          this.posting = false;
          this.posted = true;
        })
        .catch(err => {
          this.posting = false;
        });
    }
  },
  filters: {
    sentence: function(value) {
      if (!value) return "";
      value = value.toString();
      return value.charAt(0).toUpperCase() + value.slice(1).replace("-", " ");
    }
  },
  data() {
    return {
      name: "",
      posting: false,
      type: "all-time",
      posted: false
    };
  }
};
</script>

<style scoped>
.spread {
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  min-height: 100vh;
}
.grey {
  text-align: center;
  opacity: 0.4;
  padding: 1rem;
}
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
}
.mb-2 {
  margin-bottom: 2rem;
}
.mh-auto {
  margin: auto;
}
.scoreboard-tabs {
  display: flex;
  flex-direction: row;
}
.tab {
  padding: 1rem;
  cursor: pointer;
  border-radius: 2rem;
  margin: 0.5rem;
  color: inherit;
}
.tab:hover:not(.active),
.tab:focus:not(.active) {
  background-color: rgba(120, 120, 120, 0.1);
}
.active {
  background-color: var(--blue);
  color: #eee;
}
h2 {
  margin-top: 0;
  margin-bottom: 0.4rem;
}
input {
  border: none;
  padding: 0.6rem;
  font-size: 1.2rem;
  width: 20rem;
}
.col {
  display: flex;
  flex-direction: column;
  text-align: center;
  align-items: center;
  background-color: #eee;
  padding: 2rem;
  margin: 2rem 0;
  box-shadow: #9993 0 1rem 1rem;
}
.link {
  text-decoration: none;
  display: inline-block;
}
</style>