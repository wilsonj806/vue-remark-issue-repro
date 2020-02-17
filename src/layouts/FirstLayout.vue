<template>
  <div class="my-focused" v-bind:style="`border: 2px solid ${color};`">
    <slot/>
  </div>
</template>

<script>
export default {
  props: {
    color: {
      required: false,
      default: "black"
    }
  }
}
</script>

<style scoped>
.my-focused {
  padding: 0.5rem;
}
</style>