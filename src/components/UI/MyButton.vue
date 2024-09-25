<template>
  <button  @click="emitClick" class="btn">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'my-button',
  methods: {
    emitClick() {
      this.$emit('click');
    }
  }
}
</script>

<style scoped>
.btn {
  padding: 10px 15px;
  background:#2196f3;
  color: #fff;
  border-radius: 3px;
}
</style>