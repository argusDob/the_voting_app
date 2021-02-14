<template>
  <div class="home">
    <user-role  ref="modal"></user-role>
    <b-container v-if="appState" class="h-100">
          <notifier></notifier>

        <b-row>
        <b-col class="border-right mb-3">
          <questionnaire-form></questionnaire-form>     
          </b-col>
        <b-col class="border-right mb-3"><voting-form></voting-form></b-col>
        <b-col class="border-right"><voting-graphs></voting-graphs></b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import UserRole from "@/components/Shared/Modal/ModalContent.vue";
import QuestionnaireForm from "@/components/QuestionnaireForm.vue";
import VotingForm from "@/components/VotingForm.vue";
import VotingGraphs from "@/components/VotingGraphs.vue";
import Notifier from '@/components/Shared/Notifier.vue'



import { mapGetters } from "vuex";

//todo ΤΑ ΚΟΜΠΟΝΕΝΤΣ ΠΑΙΔΙΑ ΤΟΥ ΧΟΜΕ ΠΡΕΠΕΙ ΝΑ ΦΥΓΟΥΝ ΑΠΟ ΤΑ ΣΑΡΕΔ ΚΑΙ ΝΑ ΠΑΝΕΣΤΟ views

export default {
  name: "Home",
  components: {
    "user-role": UserRole,
    "questionnaire-form": QuestionnaireForm,
    "voting-form": VotingForm,
    "voting-graphs": VotingGraphs,
    "notifier": Notifier
  },

  data() {
    return {
      appState:false,
    };
  },
  mounted() {
    this.open;
    this.$refs.modal.show(this.initUserModal());
  },
    computed: {
    ...mapGetters("app", ["returnAppState"]),
   },
     watch: {
     returnAppState(pAppState) {
      this.appState = pAppState; 
     }
   },
  methods: {
    initUserModal() {
      const theUserModal = {};
      theUserModal.title = "Welcome to the voting app";
      theUserModal.buttonVariant = "success";
      theUserModal.buttonStyle = "w-50 ";
      return theUserModal;
    },
  }
};
</script>

