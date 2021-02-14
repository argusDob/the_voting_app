
<template>
  <popup-modal ref="popup">
    <div class="d-flex justify-content-center align-items-center flex-column">
      <div class="d-flex justify-content-center align-tiems-center w-100 mt-3">
        <confirm-button
          :buttonStyle="buttonStyle"
          :variantType="variantType"
          :disableButton = "disableButton"
          btnText="Start"
          @click.native="submit()"
        ></confirm-button>
      </div>
    </div>
  </popup-modal>
</template>

<script>
import PopupModal from "@/components/Shared/Modal/Modal.vue";
import ConfirmButton from "@/components/Shared/Buttons/Button.vue";
import { mapMutations, mapGetters } from "vuex";

export default {
  name: "ModalContent",
  components: {
    "popup-modal": PopupModal,
    "confirm-button": ConfirmButton
  },

  data() {
    return {
      title: undefined,
      variantType: undefined,
      buttonStyle: undefined,
      disableButton:undefined,
    };
  },

  methods: {
    ...mapMutations("app", ["SET_APP_STATE"]),
    ...mapGetters("app", ["returnAppState"]),
    show(initModal = {}) {
      this.title = initModal.title;
      this.selectOption = initModal.selectOption;
      const theButton = this.initButton(initModal);
      this.variantType = theButton.variantType;
      this.buttonStyle = theButton.buttonStyle;
      this.disableButton = theButton.disableButton;
      this.$refs.popup.open(this.title);
    },
   confirm() {
      this.$refs.popup.close();
    },
    cancel() {
      this.$refs.popup.close();
    },
    submit() {
      this.$refs.popup.close();
      this.SET_APP_STATE(true);
    },
    initButton(pInitModal){
      const button = {}
      button.variantType = pInitModal.buttonVariant;
      button.buttonStyle = pInitModal.buttonStyle;
      button.disableButton = false;
      return button
    }
  }
};
</script>