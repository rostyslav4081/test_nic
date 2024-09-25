<template>
  <div v-if="domainData" class="domain-detail-container">
    <!-- Header -->
    <header class="header">
      <h1>{{ domainData.fqdn }}</h1>
    </header>

    <!-- Toggle Verbose View -->
    <div class="toggle-container">
      <CustomCheckbox v-model="isVerboseView">Verbose view</CustomCheckbox>
    </div>

    <!-- Main Content Split into Two Columns -->
    <div class="main-content">
      <!-- Left Column -->
      <div class="left-column">
        <!-- AuthInfo Section -->
        <div class="card auth-info">
          <div class="auth">
            <span>AuthInfo:</span>
            <!-- Button to trigger modal -->
            <my-button class="show-button" @click="openModal">SHOW</my-button>
          </div>
          <p><strong>Expires at:</strong> {{ formatDate(domainData.expires_at) }}</p>
        </div>

        <!-- Events Section -->
        <div class="card events">
          <h3>Events:</h3>
          <div class="event-row">
            <p><strong>Create date:</strong> {{ formatDate(domainData.events.registered.timestamp) }}</p>
            <p><strong>Registrar:</strong> {{ domainData.events.registered.registrar_handle }}</p>
          </div>
          <div class="event-row">
            <p><strong>Update date:</strong> {{ formatDate(domainData.events.updated.timestamp) }}</p>
            <p><strong>Registrar:</strong> {{ domainData.events.updated.registrar_handle }}</p>
          </div>
          <div class="event-row">
            <p><strong>Transfer date:</strong> {{ formatDate(domainData.events.transferred.timestamp) }}</p>
            <p><strong>Registrar:</strong> {{ domainData.events.transferred.registrar_handle }}</p>
          </div>
        </div>

        <!-- State Flags Section -->
        <div class="card state-flags">
          <h3>State Flags:</h3>
          <ul class="flags-list">
            <li
                v-for="flag in filteredFlags"
                :key="flag.name"
                :class="getFlagClass(flag.active)"
            >
              <span class="flag-indicator" :class="getFlagIndicatorClass(flag.active)"></span>
              {{ flag.description }}
            </li>
          </ul>
        </div>
      </div>

      <!-- Right Column -->
      <div class="right-column">
        <!-- Owner Section -->
        <div class="card owner">
          <h3>Owner:</h3>
          <p><strong>Handle:</strong> {{ domainData.owner.handle }}</p>
          <p v-if="domainData.owner.publish.organization"><strong>Organization:</strong> {{ domainData.owner.organization }}</p>
          <p v-if="domainData.owner.publish.name"><strong>Name:</strong> {{ domainData.owner.name }}</p>
        </div>

        <!-- Administrative Contacts -->
        <div class="card contacts">
          <h3>Administrative Contacts:</h3>
          <ul>
            <li v-for="contact in domainData.administrative_contacts" :key="contact.handle">
              <p><strong>Handle:</strong> {{ contact.handle }}</p>
              <p v-if="contact.publish.organization"><strong>Organization:</strong> {{ contact.organization }}</p>
              <p v-if="contact.publish.name"><strong>Name:</strong> {{ contact.name }}</p>
            </li>
          </ul>
        </div>

        <!-- NSSet Section -->
        <div class="card nsset">
          <h3>NSSet:</h3>
          <p><strong>Handle:</strong> {{ domainData.nsset.handle }}</p>
          <p><strong>Registrar:</strong> {{ domainData.nsset.registrar }}</p>
          <p><strong>DNS:</strong></p>
          <ul>
            <li v-for="dns in domainData.nsset.dns" :key="dns.name">{{ dns.name }} ({{ dns.ip_address }})</li>
          </ul>
        </div>

        <!-- KeySet Section -->
        <div class="card keyset">
          <h3>KeySet:</h3>
          <p><strong>Handle:</strong> {{ domainData.keyset.handle }}</p>
          <p><strong>Registrar:</strong> {{ domainData.keyset.registrar }}</p>
          <p><strong>DNS Keys:</strong></p>
          <ul>
            <li v-for="key in domainData.keyset.dns_keys" :key="key">{{ key }}</li>
          </ul>
        </div>
      </div>
    </div>

    <!-- Modal Window -->
    <MyModal :isOpen="isModalOpen" @confirm="handleConfirm" @close="closeModal" />
  </div>

  <div v-else>
    <p>Loading...</p>
  </div>
</template>

<script setup>
import { ref, computed, defineProps } from 'vue';
import MyButton from '@/components/UI/MyButton.vue';
import CustomCheckbox from '@/components/UI/MyCheckbox.vue';
import MyModal from '@/components/UI/MyModal.vue'; // Import the modal component

const props = defineProps({
  domainData: Object,
});

const isVerboseView = ref(false);
const isModalOpen = ref(false); // State to control modal visibility

const openModal = () => {
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

const handleConfirm = () => {
  alert('Confirmed modal action!');
  closeModal();
};

const filteredFlags = computed(() => {
  return isVerboseView.value
      ? props.domainData.state_flags.flags // Show all flags in verbose view
      : props.domainData.state_flags.flags.filter((flag) => flag.active); // Show only active (green) flags when verbose is off
});

const getFlagClass = (isActive) => {
  return isActive ? 'flag-green' : 'flag-red';
};

const getFlagIndicatorClass = (isActive) => {
  return isActive ? 'flag-green' : 'flag-red';
};

const formatDate = (date) => {
  const options = {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
    hour: 'numeric',
    minute: 'numeric',
    second: 'numeric',
  };
  return new Date(date).toLocaleDateString(undefined, options);
};
</script>

<style scoped>
/* Basic container */
.domain-detail-container {
  max-width: 1200px;
  margin: 20px auto;
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
}

/* Header styles */
.header {
  padding: 10px 0;
  background-color: #007acc;
  color: white;
  border-radius: 8px;
  text-align: center;
}

.header h1 {
  margin: 0;
}

.auth {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

/* Main content layout */
.main-content {
  display: flex;
  justify-content: space-between;
  gap: 20px;
}

/* Left and right columns */
.left-column,
.right-column {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* Responsive behavior for smaller screens */
@media (max-width: 768px) {
  .main-content {
    flex-direction: column;
  }
}

/* Card component styling */
.card {
  background-color: white;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 8px;
}

/* Remove list decoration */
ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  list-style: none;
}

/* Flag Classes */
.flag-green {
  color: green;
}

.flag-red {
  color: red;
}

.flag-indicator {
  display: inline-block;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  margin-right: 5px;
}

.flag-indicator.flag-green {
  background-color: green;
}

.flag-indicator.flag-red {
  background-color: red;
}

/* Flags list */
.flags-list {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 5px;
}

/* Toggle styling */
.toggle-container {
  margin: 15px 0;
  display: flex;
  align-items: center;
}
</style>
