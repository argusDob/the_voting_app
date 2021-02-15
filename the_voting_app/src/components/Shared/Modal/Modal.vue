
<template>
  <b-modal
    v-model="isVisible"
    id="appModal"
    @click="open()"
    no-close-on-esc
    no-close-on-backdrop
    hide-footer
    hide-header-close
    :title="title"
    size="lg"
  >
    <slot></slot>
  </b-modal>
</template>


<script>
export default {
  name: "appModal",
  data() {
    return {
      isVisible: false,
      title: null
    };
  },
  methods: {
    open(pTitle) {
      this.isVisible = true;
      this.title = pTitle;
    },

    close() {
      this.isVisible = false;
    }
  }
};
</script>

<style>
</style>