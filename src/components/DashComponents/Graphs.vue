<template>
  <div id="moodGraphs" class="graphs text-center">
    <v-card elevation="0" :loading="loading">
      <v-card-text>
        <div class="instances-tracked">
          <InstancesTracked :instancesTracked="instancesTracked" />
        </div>

        <div class="graphs">
          <div
            v-for="mood in userProfile[0].userMoods"
            :key="mood.mood"
            class="mood"
          >
            <div
              class="single-mood"
              :class="mood.mood"
              :style="
                `width: ${Math.floor(
                  (100 / instancesTracked) * mood.timesFelt
                ) || 0}%;
            background: ${mood.color}; border-left: 4px solid ${
                  mood.borderColor
                }; `
              "
            ></div>
            <span>{{ mood.mood }} | Times felt: {{ mood.timesFelt }}</span>
          </div>
        </div>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
import InstancesTracked from "./InstancesTracked.vue";

export default {
  name: "MoodBarGraphs",

  components: {
    InstancesTracked,
  },

  props: {
    userProfile: {
      type: Array,
      required: true,
    },
  },

  computed: {
    instancesTracked() {
      return this.userProfile[0].userMoods.reduce(
        (a, { timesFelt }) => a + timesFelt,
        0
      );
    },

    loading() {
      return this.$store.getters.loading;
    },
  },
};
</script>

<style scoped>
.graphs {
  width: 75%;
  margin: 0 auto;
}

.mood {
  background: #fafafa;
  margin: 10px auto;
  position: relative;
}

.single-mood {
  height: 100%;
  position: absolute;
  box-sizing: border-box;
}

span {
  text-align: center;
  display: inline-block;
  padding: 10px 20px;
}
</style>
