<template>
  <nav class="navbar" id="navbar">
    <div class="title">
      <RouterLink to="/overview">價格追蹤小幫手</RouterLink>
      <div class="hamburger" @click="toggleOptions">☰</div>
    </div>
    <ul class="options" v-show="optionsDisplay">
      <li v-for="option in optionMenu" :key="option.id">
        <RouterLink :to="option.to">{{ option.label }}</RouterLink>
      </li>
      <li><RouterLink to="/overview">物價概覽</RouterLink></li>
      <li><RouterLink to="/trending">物價趨勢</RouterLink></li>
      <li><RouterLink to="/news">相關新聞</RouterLink></li>
      <li v-if="!isLoggedIn"><RouterLink to="/login">登入</RouterLink></li>
      <li v-else @click="logout">Hi, {{ getUserName }}! 登出</li>
    </ul>
  </nav>
</template>

<script setup>
import { computed, onMounted, ref, onBeforeUnmount } from "vue";
import { useAuthStore } from "@/stores/auth";

const optionMenu = [
  { to: "/overview", label: "物價概覽" },
  { to: "/trending", label: "物價趨勢" },
  { to: "/news", label: "相關新聞" },
];

const userStore = useAuthStore();

const isLoggedIn = computed(() => userStore.isLoggedIn);
const getUserName = computed(() => userStore.getUserName);

const logout = () => {
  userStore.logout();
};

var navbar = null;
var navbarWidth = null;
var optionsDisplay = ref(false);

onMounted(() => {
  navbar = document.getElementById("navbar");
  if (navbar) updateRwdDisplay();
  window.addEventListener("resize", updateRwdDisplay);
});

onBeforeUnmount(() => {
  window.removeEventListener("resize", updateRwdDisplay);
});

const updateRwdDisplay = () => {
  navbarWidth = navbar.offsetWidth;
  if (navbarWidth > 768) optionsDisplay.value = true;
  else {
    optionsDisplay.value = false;
  }
};

const toggleOptions = () => {
  if (optionsDisplay.value) optionsDisplay.value = false;
  else optionsDisplay.value = true;
};
</script>

<style scoped>
.navbar {
  display: flex;
  justify-content: space-between;
  background-color: #f3f3f3;
  padding: 1.5em;
  height: 4.5em;
  width: 100%;
  align-items: center;
  box-shadow: 0 0 5px #000000;
}

.navbar ul {
  list-style: none;
  display: flex;
  justify-content: space-around;
}

.title > a {
  font-size: 1.4em;
  font-weight: bold;
  color: #2c3e50 !important;
}

.navbar li {
  color: #575b5d;
  margin: 0 0.5em;
  font-size: 1.2em;
}

.navbar li:hover {
  cursor: pointer;
  font-weight: bold;
}

.navbar a {
  text-decoration: none;
  color: #575b5d;
}

.hamburger {
  display: none;
}

@media (max-width: 768px) {
  .navbar {
    flex-direction: column;
    height: auto;
    padding: 0;
  }

  .navbar ul {
    flex-direction: column;
    display: flex;
    width: 100%;
  }

  .navbar li {
    padding: 10px;
    border-top: 1px solid rgb(87, 87, 87);
    width: 100%;
    justify-content: center;
    margin: 0;
  }

  .title {
    display: flex;
    width: 100%;
    margin-left: 10px;
    align-items: center;
  }

  .hamburger {
    padding: 10px;
    display: block;
    margin-left: auto;
    cursor: pointer;
  }
}
</style>
