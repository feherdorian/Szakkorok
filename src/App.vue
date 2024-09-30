<script setup>
import { RouterLink, RouterView } from "vue-router";
import { ref, onMounted } from "vue";

const activeTheme = ref("theme-green");

const themes = ref([
  { id: "theme-green", name: "Green", color: "#37b18c" },
  { id: "theme-blue", name: "Blue", color: "#377ab1" },
  { id: "theme-red", name: "Red", color: "#b14537" },
  { id: "theme-purple", name: "Purple", color: "#8a37b1" },
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
    <header class="mb-4 text-center position-relative">
      <h1 class="display-3">
        <span class="title-text">Szakkörök</span>
      </h1>

      <button class="btn btn-theme" @click="toggleThemeWindow">
        <i class="fa fa-cog"></i>
      </button>

      <div v-if="showThemeWindow" class="theme-window">
        <div class="theme-options">
          <div
            v-for="theme in themes"
            :key="theme.id"
            :id="theme.id"
            :class="['theme-box', { active: activeTheme === theme.id }]"
            @click="setActiveTheme(theme.id, theme.color)"
            :style="{ backgroundColor: theme.color }"
          ></div>
        </div>
      </div>

      <!-- Elválasztó vonal -->
    </header>

    <ul class="nav justify-content-center mb-4">
      <li>
        <RouterLink
          :to="{ path: '/' }"
          :class="{ active: $route.path === '/' }"
        >
          <i class="fa fa-plus"></i>
        </RouterLink>
      </li>
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
  position: relative;
}

.display-3 {
  font-weight: 400;
  position: relative; /* Position relative for absolute child */
}

/* Style for the title text */
.title-text {
  display: inline-block; /* Makes the span only take up as much space as needed */
  position: relative;
}

.title-text::after {
  content: '';
  position: absolute;
  width: 100%;
  bottom: 0;
  left: 0;
  height: 2px;
  background: #fff;
}

/* Style for the border below the title */
.title-border {
  border-bottom: 2px solid #fff; /* Line below the title */
  display: block; /* Ensures it appears on a new line */
  width: 100%; /* Make it full width */
  margin-top: 10px; /* Space between title and border */
}

.btn-theme {
  background-color: var(--text-color);
  color: white;
  padding: 8px 12px;
  font-size: 16px;
  border: none;
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
}

.btn-theme i {
  font-size: 18px;
}

.btn-theme:hover {
  background-color: #777;
}

.theme-options {
  display: flex;
  justify-content: center;
  padding: 20px 0;
  flex-wrap: wrap;
}

.theme-box {
  cursor: pointer;
  width: 30px;
  height: 30px;
  border-radius: 4px;
  margin: 10px;
  opacity: 0.6;
  transition: all 0.3s;
}

.theme-box.active {
  opacity: 1;
}

.theme-window {
  position: absolute;
  top: 50px;
  right: 10px;
  background-color: var(--bg-black-50);
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 20px;
  width: 160px;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
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

.nav li a.active {
  color: var(--text-color);
  font-weight: 700;
}

.form-check-input:checked {
  background-color: var(--text-color);
  border-color: var(--text-color);
}
</style>
