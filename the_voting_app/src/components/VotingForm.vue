<template>
  <div>
    <b-container>
      <h4>{{ theQuestion }}</h4>
      <div class="d-flex align-content-start">
        <div class="d-flex">
          <b-form-checkbox-group
            id="checkbox-group-1"
            v-model="selected"
            :options="options"
            name="flavour-1"
            class="d-flex flex-column mx-auto"
          ></b-form-checkbox-group>
        </div>
      </div>
      <b-row>
        <confirm-button v-if="isNew" variantType="success" btnText="Vote" @click.native="submit()"></confirm-button>
      </b-row>
    </b-container>
  </div>
</template>

<script>
//TODO 
import { mapGetters, mapMutations } from "vuex";
import ConfirmButton from "@/components/Shared/Buttons/Button.vue";

export default {
  name: "VotingForm",
  components: {
    "confirm-button": ConfirmButton
  },
  data() {
    return {
      questionnaire: undefined,
      theQuestion: undefined,
      isNew: false,
      theAnswers: [],
      selected: [],
      options: [],
      arr: [], 
      list: [],
    };
  },
  computed: {
    ...mapGetters("voting", ["returnQuestionnaire"])
  },
  watch: {
    returnQuestionnaire(pNewQuestionnaire, pOldQuestionnaire) {
      if (typeof pNewQuestionnaire !== "undefined") {
        this.initVotingForm(pNewQuestionnaire);
      }
      if (typeof pNewQuestionnaire !== "undefined" && typeof pOldQuestionnaire !== "undefined") {
        this.resetTheComponent(pOldQuestionnaire, pNewQuestionnaire);
      }
    }
  },
  methods: {
    ...mapMutations("voting", ["TRACK_VOTES", "RESET_STATE"]),
    getQuestionsAndAnswers(pQuestionnaire) {
      this.options.length = 0;
      pQuestionnaire.forEach(questionnaire => {
        this.theQuestion = questionnaire.question;
        delete questionnaire.question;
        this.fillInCheckBoxes(questionnaire);
      });
      return true;
    },
    initVotingForm(pNewQuestionnaire) {
      if (pNewQuestionnaire.length) {
        const theQuestionnaire = [...pNewQuestionnaire];
        this.getQuestionsAndAnswers(theQuestionnaire);
        this.isNew = true;
      }
    },
    resetTheComponent(pOldQuestionnaire, pNewQuestionnaire) {
      if (pOldQuestionnaire.length && !pNewQuestionnaire.length) {
        this.theQuestion = "";
        this.theAnswers = [];
        this.options = [];
        this.arr = [];
        this.list = [];
        this.isNew = false;
        this.selected = [];
        this.initButton();
      }
    },
    initButton() {
      this.variantType = "success";
    },
    fillInCheckBoxes(pQuestionnaire) {
      for (let answers in pQuestionnaire) {
        const theOption = {};
        theOption.text = pQuestionnaire[answers];
        theOption.value = pQuestionnaire[answers];
        this.options.push(theOption);
      }
    },
    submit() {
      this.selected.forEach(votes => {
        this.arr.push(votes);
      });
      this.countVotes(this.arr);
    },
    countVotes(pArr) { //TODO
      const theRepeatedList = [];
      let theRepeatedValues = {};
      pArr.forEach(function(x) {
        theRepeatedValues[x] = (theRepeatedValues[x] || 0) + 1;
      });
      theRepeatedList.push(theRepeatedValues);
      this.list = theRepeatedList;
      this.TRACK_VOTES(this.list);
    }
  }
};
</script>

