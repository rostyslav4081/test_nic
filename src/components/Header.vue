<template>
  <div class="header">
    <!-- Burger Menu Component -->
    <my-burger-menu />

    <!-- User Content Section (Click to show dropdown for 15 seconds) -->
    <div class="user-menu">
      <div class="user-content" @click="toggleMenu">
        <user-logo />
        <div class="admin-name">JAN MASILEK</div>
      </div>

      <!-- Dropdown Menu -->
      <div v-if="menuVisible" class="dropdown-menu">
        <p>JAN MASILEK</p>
        <p class="user-role">admin</p>
        <hr />
        <button @click="logout">Logout</button>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import UserLogo from "@/components/UI/UserLogo.vue";
import MyBurgerMenu from "@/components/UI/MyMenu.vue";

export default defineComponent({
  name: "Header",
  components: {
    MyBurgerMenu,
    UserLogo,
  },
  data() {
    return {
      menuVisible: false, // Control dropdown visibility
      timeoutId: null, // Store the timeout ID
    };
  },
  methods: {
    toggleMenu() {
      // Show the dropdown menu and start the timer to hide it after 15 seconds
      this.menuVisible = true;

      // Clear any existing timeout before setting a new one
      if (this.timeoutId) {
        clearTimeout(this.timeoutId);
      }

      // Set timeout to hide the menu after 15 seconds
      this.timeoutId = setTimeout(() => {
        this.menuVisible = false;
      }, 15000); // 15 seconds
    },
    logout() {
      alert("Logging out...");
      // Implement the logout functionality here
    },
  },
  beforeUnmount() {
    // Clear the timeout if the component is destroyed before the timer finishes
    if (this.timeoutId) {
      clearTimeout(this.timeoutId);
    }
  },
});
</script>

<style scoped>
.header {
  background-color: #2196f3;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center; /* Vertically align items */
  color: #fff;
  padding: 1rem;
  width: 100%;
}

.user-menu {
  position: relative;
}

.user-content {
  display: flex;
  flex-direction: row;
  gap: 1rem;
  align-items: center; /* Ensure items are vertically aligned */
  cursor: pointer;
}

.admin-name {
  font-weight: bold;
}

/* Dropdown menu styling */
.dropdown-menu {
  position: absolute;
  right: 0;
  top: 3rem;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  padding: 10px;
  z-index: 1000;
  color: black;
}

.dropdown-menu p {
  margin: 5px 0;
}

.user-role {
  color: #888;
}

.dropdown-menu hr {
  margin: 10px 0;
  border: none;
  border-top: 1px solid #ddd;
}

.dropdown-menu button {
  width: 100%;
  padding: 8px;
  background-color: #007acc;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.dropdown-menu button:hover {
  background-color: #005f99;
}
</style>
