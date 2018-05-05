<template>
  <div class="container">
    <div class="scoreboard-container">
			<div class="scoreboard-tabs container">
				<div v-for="tab in ['last', 'daily', 'weekly', 'all-time']" :key="tab" class="tab" :class="{ active: type == tab }" @click="type = tab">
					{{ tab | sentence }}
				</div>
			</div>
			<div>
				<score-board class="mh-auto scoreboard" :type="type" ref="scoreboard" />
			</div>
    </div>
    <div class="container">
      <div>
        <router-link to="/play" class="link"><button class="button">Play</button></router-link>
      </div>
    </div>
    <div class="mt col" v-if="registeredTime > 0">
			<div>
				<h2>Your result</h2>
				<p>Your time was {{ registeredTime }} seconds</p>
			</div>
			<input v-model="name" class="number-input" type="text" autofocus="true" placeholder="Enter a name" />
			<button class="button" @click="postResult">Post</button>
    </div>
		<div class="grey"><span>Made by Empinini ;)</span></div>
  </div>
</template>

<script>
import ScoreBoard from "@/components/ScoreBoard";
import BInput from "@/components/Input";

export default {
  components: {
    ScoreBoard,
    BInput
  },
  props: {
    registeredTime: Number,
    max: Number,
    min: Number
  },
  methods: {
    postResult() {
      if (this.posting) return;

      this.$http
        .post("/scores", {
          time: this.registeredTime,
          max: this.max,
          min: this.min,
          name: this.name
        })
        .then(d => {
          this.registeredTime = null;
          this.posting = true;
          this.$refs.scoreboard.fetchScoreboard();
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
      type: "all-time"
    };
  }
};
</script>

<style scoped>
.grey {
  text-align: center;
  bottom: 2em;
  width: 100%;
  position: absolute;
  opacity: 0.4;
}
.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  justify-items: center;
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
  margin: 0.5rem;
  cursor: pointer;
  border-radius: 1em;
}
.tab:hover:not(.active) {
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

.button {
  /* text-transform: uppercase; */
  padding: 1rem 2rem;
  background-color: var(--blue);
  border-radius: 0.8rem;
  color: #eee;
  letter-spacing: 0.2rem;
  border: none;
  font-size: 1.1rem;
  cursor: pointer;
  box-shadow: 0 0.5rem var(--blue-dark);
  margin-bottom: 0.8rem;
  margin-top: 1rem;
  transition: all 100ms ease;
}
.button:hover {
  transform: translateY(0.5rem);
  box-shadow: 0 0 var(--blue-dark);
}
.button:active:hover {
  background-color: var(--blue-dark);
}

.link {
  text-decoration: none;
}
.mt {
  margin-top: 1rem;
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
  padding: 2em;
}
</style>