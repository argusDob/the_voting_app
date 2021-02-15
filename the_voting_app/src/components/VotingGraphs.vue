<template>
  <div v-if="isNew">
    <vue-bar-graph
      :points="points"
      :show-y-axis="showYaxis"
      :show-x-axis="showXaxis"
      :width="height"
      :height="width"
      :show-values="showValues"
    />
    <div v-if="totalVotes !== null" class="mt-2">
     <strong>Total Votes: </strong>{{ totalVotes }}
    </div>
  </div>
</template>

<script>
import PureVueChart from "pure-vue-chart";
import { mapGetters } from "vuex";

export default {
  name: "VotingGraphs",
  components: {
    "vue-bar-graph": PureVueChart
  },
  data() {
    return {
      //   points:[{label: 'N', value: 41.1}, {label: 'NW', value: 1}, {label: 'W', value: 15}]
      points: [],
      height: 200,
      width: 400,
      showXaxis: true,
      showYaxis: false,
      showValues: true,
      isNew: true,
      totalVotes:null
    };
  },
  computed: {
    ...mapGetters("voting", ["returnVotes"])
  },
  watch: {
    returnVotes(pNewVotes, pOldVotes) {
      if(typeof(pNewVotes) !== "undefined") { (this.initTheComponent(pNewVotes)); }
      if((typeof(pNewVotes) !== "undefined") && (typeof(pOldVotes) !== "undefined")) {
      this.resetTheComponent(pOldVotes, pNewVotes);
       }
    }
  },
  methods: {
    getPoints(pVotes) {
      const thePoints = [];
      if(typeof(thePoints) !== "undefined"){
      pVotes.forEach(votes => {
        for (let vote in votes) {
          let thePoint = {};
          thePoint.value = votes[vote];
          thePoint.label = vote;
          thePoints.push(thePoint);
        }
      });
      }
      this.points = thePoints;
      this.calculatePoints(thePoints)
      return thePoints;
    },
     calculatePoints(pPoints){
       let totalVotes = 0;
       pPoints.forEach(votes => {
          totalVotes += votes.value
       })
       this.totalVotes = totalVotes
     },
    initTheComponent(pNewVotes) {
      if (pNewVotes.length) {
        let theVotes = [...pNewVotes];
        this.getPoints(theVotes);
        this.isNew = true;
      }
    },
    resetTheComponent(pOldVotes, pNewVotes) {
      if (pOldVotes.length === 1 && pNewVotes.length === 0) {
        this.isNew = false;
        this.totalVotes = null;
      }
    }
  }
};
</script>

