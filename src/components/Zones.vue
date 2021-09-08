<template>
  <div class="available-zones">
    <div>
      <label>
        <p>
          Available zones
        </p>
        <select v-model="zone" class="select default-el">
          <option v-for="(zone, index) in zones" :value="zone" :key="index"
            >{{ zone }}
          </option>
        </select>
      </label>

      <div class="get-zones">
        <button @click="updateTime" class="btn default-el">
          Get current time
        </button>

        <button @click="addTime" class="btn default-el ml-1">
          +
        </button>
      </div>
    </div>
  </div>
</template>

<script>
const axios = require("axios");
import timeDetails from '@/mixins/timeDetails.js'

export default {
  name: "WorldTime",
  mixins: [timeDetails],
  data() {
    return {
      zones: [],
    };
  },
  mounted() {
    axios.get("http://worldtimeapi.org/api/timezone").then((res) => {
      this.zones = res.data;
    });
  },
  methods: {
    addTime() {
      this.getTimeDetails().then((timeDetails) => {
        this.$emit("add-time", timeDetails);
      });
    },
    updateTime() {
      this.getTimeDetails().then((timeDetails) => {
        this.$emit("update-time", timeDetails);
      });
    },
  },
};
</script>

<style lang="scss" scoped>
.available-zones {
  margin-top: 20px;
}

.get-zones {
  margin-top: 10px;
}
</style>
