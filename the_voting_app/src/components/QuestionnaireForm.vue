<template>
  <div>
    <b-container>
      <form @submit.prevent="validateBeforeSubmit">
        <b-row>
          <label for="input-small">
            <strong>The question:</strong>
          </label>
          <b-col sm="10">
            <b-form-input v-model="theQuestion" :disabled="theQuestion.length > 80" placeholder="Please, type your question."></b-form-input>
          </b-col>
        </b-row>
        <b-row class="mt-4">
          <b-col sm="10">
            <b-form-input v-model="answer1" :disabled="answer1.length > 80" placeholder="Please, type your answer."></b-form-input>
          </b-col>
        </b-row>
        <b-row class="mt-2">
          <b-col sm="10">
            <b-form-input v-model="answer2" :disabled="answer2.length > 80" placeholder="Please, type your answer."></b-form-input>
          </b-col>
        </b-row>
        <b-row class="mt-2" v-for="(item, i) in items" :key="i">
          <b-col sm="10 d-flex">
            <b-form-input class="w-20"  v-focus  v-model="items[i]" :disabled="item.length  > 80" placeholder="Please, type your answer."> </b-form-input>
            <my-button
              v-if="i === items.length-1"
              variantType="danger"
              btnText="Remove"
              @click.native="removeField()"
            ></my-button>
          </b-col>
        </b-row>
        <b-row class="mt-2">
          <b-col class="d-flex justify-content-around">
            <my-button variantType="info" btnText="Add" @click.native="addInput()"></my-button>
            <my-button type="submit" variantType="success" btnText="Submit"></my-button>
            <my-button variantType="warning" btnText="Reset" @click.native="resetState()"></my-button>
          </b-col>
        </b-row>
      </form>
    </b-container>
  </div>
</template>
<script>

import { mapMutations } from "vuex";
import ConfirmButton from "@/components/Shared/Buttons/Button.vue";

export default {
  name: "VotingForm",
  components: {
    "my-button": ConfirmButton
  },
  data() {
    return {
      theQuestion: "",
      items: [],
      inputsAmount: 0,
      answer1: "",
      answer2: "",
      form: [],
      disableInput: false
    };
  },
  directives: {
  focus: {
    inserted: function (el) {
      el.focus()
    },  
  }
},
  methods: {
    ...mapMutations("voting", ["SET_QUESTIONNAIRE", "TRACK_VOTES"]),
    ...mapMutations("notification", ["notify", "setStatus"]),

    addInput() {
      const staticFieldsHasValue = !this.checkStaticFieldsAreEmpty();
      if (staticFieldsHasValue) {
        if (this.items.length < 8) {
             this.items.push("");
        }
      } else {
        this.notifications("Please fill in first the above inputs", "warning");
      }
    },
    getFormsInputs() {
      let theFormsInputs = {},
      theQuestionnaire = [],
      overLimit = false;
      theFormsInputs.question = this.theQuestion;
      theFormsInputs.answer1 = this.answer1;
      theFormsInputs.answer2 = this.answer2;
      if (this.items.length) {
        this.items.forEach((inputValues, iInputValues) => {
          theFormsInputs["answer" + (iInputValues + 3)] = inputValues;
          overLimit = this.checkInputLenght(inputValues, 80);
        });
      }
      if (!overLimit) { theQuestionnaire.push(theFormsInputs);}
      return theQuestionnaire;
    },
    validateBeforeSubmit() { this.submit(); },
    submit() {
      const theQuestionnaire = this.getFormsInputs();
      const isDynamicInputsEmpty = this.checkFormForEmptyDynamicFields(theQuestionnaire);
      if (this.checkStaticFieldsAreEmpty()) {
        this.notifications("All fields are mandatory", "warning");
        return true;
      }else if((isDynamicInputsEmpty)) { this.notifications("All fields are mandatory", "warning")
       this.notifications("All fields are mandatory", "warning");
        return true;
      }else{
      this.notifications("Please answer to the following question " + this.theQuestion, "success");
      this.SET_QUESTIONNAIRE(theQuestionnaire);
      return true;
      }
    },
    resetState() {
      const isConfirmed = confirm("This action is irreversible. Are you sure you want to continue?");
      if(isConfirmed){
      this.SET_QUESTIONNAIRE([]);
      this.TRACK_VOTES([]);
      this.resetForm();
      }else{ return; }
    },
    checkInputLenght(pInputValues, pLength) {
      if (pInputValues.length > pLength) {
        console.log(pInputValues.length)
        this.notifications("Answers cannot be longer than 80 characters", "warning");
        this.disableInput = true;
        return true;
      }
      if (pInputValues.length === 0) {
        this.notifications("All fields are mandatory", "warning");
        return true;
      }
    },
    checkStaticFieldsAreEmpty() {
      if ( this.theQuestion === "" || this.answer1 === "" || this.answer2 === "") {
        return true;
      }
    },
    checkFormForEmptyDynamicFields(pTheQuestionnaire){
      if(pTheQuestionnaire.length === 0){
        return true;
      }

    },
    resetForm() {
      this.theQuestion = "";
      this.answer1 = "";
      this.answer2 = "";
      this.items = [];
      this.notifications("","");
   },
    removeField() {
      this.items.pop();
    },
    notifications(pMsg, pType) {
      this.notify({ msg: pMsg, type: pType });
    }
  }
};
</script>

