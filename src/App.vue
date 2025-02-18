<script setup>
import { ref, onMounted } from 'vue'
import DemoPage from './components/DemoPage.vue'

const theme = ref('pastel')

onMounted(() => {
  document.documentElement.setAttribute('data-theme', theme.value)
})

const handleThemeChange = (command) => {
  theme.value = command
  document.documentElement.setAttribute('data-theme', theme.value)
}
</script>

<template>
  <div class="app">
    <el-header class="nav--top">
      <h1>UI Component Library</h1>
      <el-dropdown @command="handleThemeChange">
        <el-button type="primary">
          Theme: {{ theme }} <el-icon class="el-icon--right"><arrow-down /></el-icon>
        </el-button>
        <template #dropdown>
          <el-dropdown-menu>
            <el-dropdown-item command="pastel">Pastel</el-dropdown-item>
            <el-dropdown-item command="light">Light</el-dropdown-item>
            <el-dropdown-item command="dark">Dark</el-dropdown-item>
            <el-dropdown-item command="vibrant">Vibrant</el-dropdown-item>
          </el-dropdown-menu>
        </template>
      </el-dropdown>
    </el-header>

    <div class="layout">
      <DemoPage />
      <el-aside class="sidebar-example">
        <el-menu class="nav--side">
          <el-menu-item index="1">Home</el-menu-item>
          <el-menu-item index="2">About</el-menu-item>
          <el-menu-item index="3">Contact</el-menu-item>
        </el-menu>
      </el-aside>
    </div>
  </div>
</template>

<style lang="scss">
@import './styles/styles.scss';

.app {
  min-height: 100vh;
}

.layout {
  display: flex;
  gap: 1rem;
  max-width: 1400px;
  margin: 0 auto;
  padding: 1rem;
}

.sidebar-example {
  width: 300px;
  position: sticky;
  top: 1rem;
  height: fit-content;
}

.nav--top {
  background: var(--bg-primary);
  padding: 1rem;
  border-bottom: 1px solid var(--border-color);
  display: flex;
  align-items: center;
  gap: 1rem;
}

// Override Element Plus styles to match our theme
.el-button {
  &--primary {
    --el-button-bg-color: var(--primary-color);
    --el-button-border-color: var(--primary-color);
    --el-button-hover-bg-color: var(--primary-hover);
    --el-button-hover-border-color: var(--primary-hover);
  }
}

.el-card {
  --el-card-border-color: var(--border-color);
  --el-card-bg-color: var(--bg-primary);
}

.el-input {
  --el-input-border-color: var(--border-color);
  --el-input-bg-color: var(--bg-primary);
  --el-input-text-color: var(--text-primary);
}

.el-menu {
  --el-menu-bg-color: var(--bg-secondary);
  --el-menu-text-color: var(--text-primary);
  --el-menu-hover-bg-color: var(--bg-tertiary);
  border-right: 1px solid var(--border-color);
}
</style>