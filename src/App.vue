<script setup>
import { RouterLink, RouterView } from "vue-router";
import { ref, onMounted } from "vue";

const activeTheme = ref("theme-green");

const themes = ref([
  { id: "theme-green", name: "Green", color: "#37b18c" },
  { id: "theme-greenplus", name: "Greenplus", color: "#09db2c" },
  { id: "theme-blue", name: "Blue", color: "#377ab1" },
  { id: "theme-blue2", name: "Blue2", color: "#6083b5" },
  { id: "theme-red", name: "Red", color: "#b14537" },
  { id: "theme-red2", name: "Red2", color: "#fc0505" },
  { id: "theme-purple", name: "Purple", color: "#8a37b1" },
  { id: "theme-orange", name: "Orange", color: "#b17337" },
  { id: "theme-pink", name: "Pink", color: "#b13787" },
  { id: "theme-yellow", name: "Yellow", color: "#b1a137" },
  { id: "theme-teal", name: "Teal", color: "#37b1a0" },
  { id: "theme-teal2", name: "Teal2", color: "#05fc81" },
]);

const showThemeWindow = ref(false);

const toggleThemeWindow = () => {
  showThemeWindow.value = !showThemeWindow.value;
};

const setActiveTheme = (themeId, color) => {
  document.documentElement.style.setProperty("--text-color", color);
  activeTheme.value = themeId;
  localStorage.setItem("themeColor", color);
  localStorage.setItem("themeId", themeId);
  showThemeWindow.value = false;
};

onMounted(() => {
  const savedColor = localStorage.getItem("themeColor");
  const savedThemeId = localStorage.getItem("themeId");

  if (savedColor && savedThemeId) {
    document.documentElement.style.setProperty("--text-color", savedColor);
    activeTheme.value = savedThemeId;
  }
});
</script>

<template>
  <div class="container-fluid p-4">
    <header class="mb-4 text-center">
      <h1 class="display-3">Szakkörök</h1>

      <button class="btn btn-theme" @click="toggleThemeWindow">
        Choose Theme
      </button>

      <!-- Theme selection window (dropdown-style) -->
      <div v-if="showThemeWindow" class="theme-window">
        <div class="theme-options">
          <div
            v-for="theme in themes"
            :key="theme.id"
            :id="theme.id"
            :class="{ active: activeTheme === theme.id }"
            @click="setActiveTheme(theme.id, theme.color)"
            :style="{ backgroundColor: theme.color }"
          ></div>
        </div>
      </div>
    </header>

    <ul class="nav justify-content-center mb-4">
      <li>
        <RouterLink
          :to="{ path: '/' }"
          :class="{ active: $route.path === '/' }"
        >
          <i class="fa fa-plus"></i> Szakkörök
        </RouterLink>
      </li>
      
      <li></li>
    </ul>

    <div class="content-area border rounded p-4">
      <RouterView />
    </div>
  </div>
</template>

<style>
body {
  background: var(--bg-black-100);
  height: 100%;
  user-select: none;
}

header {
  border-bottom: 2px solid #fff;
}

.display-3 {
  font-weight: 400;
}

h1,
h2,
h3,
h4,
h5,
span {
  transition: color 0.5s ease !important;
}

div {
  transition: box-shadow 0.5s ease !important;
}

button {
  transition: background 0.5s ease !important;
}

.nav li a {
  font-size: 16px;
  font-weight: 600;
  display: block;
  color: var(--text-black-700);
  padding: 5px 15px;
  text-decoration: none;
  transition: 0.5s;
}

.nav li a:hover {
  color: var(--text-color);
}

.nav li a.active,
.green {
  color: var(--text-color);
  font-weight: 700;
}

.form-check-input:checked {
  background-color: var(--text-color);
  border-color: var(--text-color);
}

.white {
  color: #fff;
}

.theme-options {
  display: flex;
  justify-content: center;
  padding: 20px 0;
  flex-wrap: wrap;
}

.theme-options div {
  cursor: pointer;
  width: 20px;
  height: 20px;
  border-radius: 4px;
  margin: 10px;
  display: inline-block;
  opacity: 0.3;
  transition: all 0.3s;
}

.theme-options div.active {
  opacity: 1;
}

.theme-window {
  position: absolute;
  top: 100px;
  right: 20px;
  background-color: var(--bg-black-50);
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 20px;
  width: 240px;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
}

.btn-theme {
  background-color: var(--bg-black-50);
  color: white;
  padding: 8px 12px;
  font-size: 16px;
  border: none;
  cursor: pointer;
}

.btn-theme:hover {
  background-color: #777;
}
</style>
