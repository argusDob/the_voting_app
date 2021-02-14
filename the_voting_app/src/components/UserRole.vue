<template>
  <div>
      <b-form-checkbox-group
        id="checkbox-group-1"
        :selected="selected"
        :options="options"
        @change="$emit('input', $event.target.options)"
        name="flavour-1"
      ></b-form-checkbox-group>
  </div>
</template>
<script>
export default {
  name: 'HelloWorld',
   props: {
    options: {
      type: Array
    },
    selected: {
      type: Array
    },

   }
}  
</script>

