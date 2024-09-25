<template>
  <div v-if="isOpen" class="modal-backdrop">
    <div class="modal">
      <p>Are you sure you want to view the modal window?</p>
      <div class="modal-actions">
        <button @click="confirm" class="confirm-button">Yes</button>
        <button @click="close" class="cancel-button">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MyModal',
  props: {
    isOpen: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    confirm() {
      this.$emit('confirm');
    },
    close() {
      this.$emit('close');
    }
  }
};
</script>

<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  text-align: center;
}

.modal-actions {
  margin-top: 20px;
  display: flex;
  justify-content: space-around;
}

.confirm-button {
  background-color: #28a745;
  color: white;
  padding: 10px 20px;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.cancel-button {
  background-color: #dc3545;
  color: white;
  padding: 10px 20px;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.confirm-button:hover {
  background-color: #218838;
}

.cancel-button:hover {
  background-color: #c82333;
}
</style>
