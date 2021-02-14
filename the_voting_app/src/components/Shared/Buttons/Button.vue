<template>
  <b-button :type="type" :class="buttonStyle"  :disabled="disableButton"  :variant="variantType">{{btnText}}</b-button>
</template>

<script>
export default {
     props: {
    variantType: {
      type: String
    },
    buttonStyle: {
      type: String
    },
    disableButton: {
      type: Boolean
    },
    btnText: {
      type: String
    },
    type: {
      type: String
    }
  },
};
</script>