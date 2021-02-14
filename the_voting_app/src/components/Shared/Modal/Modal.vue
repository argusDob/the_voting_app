
<template>
<b-modal v-model="isVisible"  id="appModal" @click="open()" no-close-on-esc  no-close-on-backdrop hide-footer hide-header-close :title="title"  size="lg">
    <slot></slot>
 </b-modal>
</template>


<script>
export default {
    name: 'appModal',
  data() {
    return {
        isVisible:false,
        title:null,
    };
  },
  methods: {
        open(pTitle) {
            this.isVisible = true;
            this.title = pTitle;
        },

        close() {
            this.isVisible = false
        }
    },
}
</script>

<style>
.modal-fullscreen .modal-dialog {
  max-width: 100%;
  margin: 0;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  height: 100vh;
  display: flex;
  position: fixed;
  z-index: 100000;
  max-width: 100%;
}
</style>