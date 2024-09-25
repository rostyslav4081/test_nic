<template>
  <label class="custom-checkbox-container">
    <slot></slot> <!-- Text first -->
    <input type="checkbox" v-model="isChecked" @change="$emit('update:modelValue', isChecked)" />
    <span class="custom-checkbox"></span> <!-- Then the checkbox -->
  </label>
</template>

<script>
export default {
  name: 'CustomCheckbox',
  props: {
    modelValue: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      isChecked: this.modelValue,
    };
  },
  watch: {
    modelValue(val) {
      this.isChecked = val;
    },
  },
};
</script>

<style scoped>
/* Custom checkbox styles */
.custom-checkbox-container {
  display: inline-flex;
  align-items: center;
  cursor: pointer;
  user-select: none;
}

.custom-checkbox-container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

.custom-checkbox {
  margin-left: 10px; /* Space between text and checkbox */
  position: relative;
  height: 20px;
  width: 20px;
  background-color: #ccc;
  border-radius: 5px;
}

.custom-checkbox-container input:checked + .custom-checkbox {
  background-color: #2196f3;
}

.custom-checkbox:after {
  content: '';
  position: absolute;
  display: none;
}

.custom-checkbox-container input:checked + .custom-checkbox:after {
  display: block;
}

.custom-checkbox:after {
  left: 7px;
  top: 3px;
  width: 6px;
  height: 12px;
  border: solid white;
  border-width: 0 3px 3px 0;
  transform: rotate(45deg);
}
</style>
