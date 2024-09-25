<template>
  <div>
    <Header/>
    <DomainDetail :domainData="domainData" />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import DomainDetail from '@/components/DomainDetail.vue';
import Header from "@/components/Header.vue";

const domainData = ref(null);

async function fetchDomainData() {
  try {
    const response = await axios.get('domain-detail.json');
    domainData.value = response.data;
  } catch (error) {
    console.error('Error fetching domain data:', error);
  }
}

onMounted(() => {
  fetchDomainData();
});
</script>
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Ferda', sans-serif;
}
.app {
  padding: 20px;
}
</style>