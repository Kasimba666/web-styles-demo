<script setup lang="ts">
import { ref, onMounted } from 'vue'

const theme = ref('pastel')
const showDropdown = ref(false)
const contextMenuPos = ref({ x: 0, y: 0 })
const showContextMenu = ref(false)

// Form state
const textInput = ref('')
const passwordInput = ref('')
const textareaInput = ref('')
const numberInput = ref(0)
const selectValue = ref('')
const radioValue = ref('')
const checkboxGroup = ref([])

onMounted(() => {
  document.documentElement.setAttribute('data-theme', theme.value)
  // Add click event listener to close context menu when clicking outside
  document.addEventListener('click', (e) => {
    if (!e.target?.closest('.context-menu-area')) {
      showContextMenu.value = false
    }
  })
})

const toggleTheme = () => {
  const themes = ['pastel', 'light', 'dark', 'vibrant']
  const currentIndex = themes.indexOf(theme.value)
  const nextIndex = (currentIndex + 1) % themes.length
  theme.value = themes[nextIndex]
  document.documentElement.setAttribute('data-theme', theme.value)
}

const toggleDropdown = () => {
  showDropdown.value = !showDropdown.value
}

const handleContextMenu = (e: MouseEvent) => {
  e.preventDefault()
  showContextMenu.value = true
  // Adjust position to prevent menu from going off-screen
  const x = Math.min(e.clientX, window.innerWidth - 160) // 160px is approximate menu width
  const y = Math.min(e.clientY, window.innerHeight - 120) // 120px is approximate menu height
  contextMenuPos.value = { x, y }
}

const closeContextMenu = () => {
  showContextMenu.value = false
}
</script>

<template>
  <div class="app">
    <el-header class="nav--top">
      <h1>UI Component Library</h1>
      <el-button type="primary" @click="toggleTheme">
        Toggle Theme ({{ theme }})
      </el-button>
    </el-header>

    <div class="layout">
      <div class="main-content">
        <!-- Panels -->
        <el-card>
          <template #header>
            <h2>Panels</h2>
          </template>
          <el-space direction="vertical" :size="16" fill>
            <el-card shadow="never" class="panel panel--soft">Soft Panel</el-card>
            <el-card shadow="hover" class="panel panel--medium">Medium Panel</el-card>
            <el-card shadow="always" class="panel panel--sharp">Sharp Panel</el-card>
          </el-space>
        </el-card>

        <!-- Buttons -->
        <el-card>
          <template #header>
            <h2>Buttons</h2>
          </template>
          <el-space wrap>
            <el-button type="primary">Primary</el-button>
            <el-button>Secondary</el-button>
            <el-button type="primary" plain>Primary Outline</el-button>
            <el-button plain>Secondary Outline</el-button>
            <el-button circle><el-icon><Search /></el-icon></el-button>
            <el-switch v-model="theme" active-value="dark" inactive-value="light" />
          </el-space>
        </el-card>

        <!-- Menus -->
        <el-card>
          <template #header>
            <h2>Menus</h2>
          </template>

          <!-- Dropdown Menu -->
          <el-dropdown trigger="click">
            <el-button type="primary">
              Open Dropdown Menu
            </el-button>
            <template #dropdown>
              <el-dropdown-menu>
                <el-dropdown-item>Regular Item</el-dropdown-item>
                <el-dropdown-item>🌟 Item with Icon</el-dropdown-item>
                <el-dropdown-item divided>Submenu Item</el-dropdown-item>
              </el-dropdown-menu>
            </template>
          </el-dropdown>

          <!-- Context Menu -->
          <el-card class="mt-4 context-menu-area" @contextmenu="handleContextMenu">
            <el-text>Right-click here for context menu</el-text>
            <el-dropdown v-if="showContextMenu" :hide-on-click="true" :teleported="false">
              Context Menu
              <div class="context-menu-trigger" :style="{
                position: 'fixed',
                left: contextMenuPos.x + 'px',
                top: contextMenuPos.y + 'px',
                width: '1px',
                height: '1px'
              }"></div>
              <template #dropdown>
                <el-dropdown-menu>
                  <el-dropdown-item>Cut</el-dropdown-item>
                  <el-dropdown-item>Copy</el-dropdown-item>
                  <el-dropdown-item>Paste</el-dropdown-item>
                </el-dropdown-menu>
              </template>
            </el-dropdown>
          </el-card>
        </el-card>

        <!-- Forms -->
        <el-card>
          <template #header>
            <h2>Form Elements</h2>
          </template>

          <el-form label-position="top">
            <h3>Text Inputs</h3>
            <el-space direction="vertical" :size="16" fill>
              <el-input v-model="textInput" placeholder="Text input" />
              <el-text>Current value: {{ textInput }}</el-text>

              <el-input v-model="passwordInput" type="password" placeholder="Password" show-password />
              <el-text>Current value: {{ passwordInput }}</el-text>

              <el-input v-model="textareaInput" type="textarea" placeholder="Textarea" />
              <el-text>Current value: {{ textareaInput }}</el-text>

              <el-input-number v-model="numberInput" placeholder="Number" />
              <el-text>Current value: {{ numberInput }}</el-text>
            </el-space>

            <h3 class="mt-4">Select</h3>
            <el-select v-model="selectValue" placeholder="Select">
              <el-option value="1" label="Option 1" />
              <el-option value="2" label="Option 2" />
              <el-option value="3" label="Option 3" />
            </el-select>
            <el-text>Selected value: {{ selectValue }}</el-text>

            <h3 class="mt-4">Radio Buttons</h3>
            <el-radio-group v-model="radioValue">
              <el-radio label="1">Option 1</el-radio>
              <el-radio label="2">Option 2</el-radio>
            </el-radio-group>
            <el-text>Selected radio: {{ radioValue }}</el-text>

            <h3 class="mt-4">Checkboxes</h3>
            <el-checkbox-group v-model="checkboxGroup">
              <el-checkbox label="1">Option 1</el-checkbox>
              <el-checkbox label="2">Option 2</el-checkbox>
            </el-checkbox-group>
            <el-text>Selected checkboxes: {{ checkboxGroup }}</el-text>
          </el-form>
        </el-card>

        <!-- Typography -->
        <el-card>
          <template #header>
            <h2>Typography</h2>
          </template>

          <el-space direction="vertical" :size="16" fill>
            <h1>Heading 1</h1>
            <h2>Heading 2</h2>
            <h3>Heading 3</h3>
            <h4>Heading 4</h4>
            <h5>Heading 5</h5>
            <h6>Heading 6</h6>

            <el-text>This is a paragraph with some sample text. It demonstrates the typography styles and how they look in different themes.</el-text>

            <el-space direction="vertical" :size="8">
              <el-text>Unordered list:</el-text>
              <ul>
                <li>Unordered list item 1</li>
                <li>Unordered list item 2</li>
                <li>Unordered list item 3</li>
              </ul>
            </el-space>

            <el-space direction="vertical" :size="8">
              <el-text>Ordered list:</el-text>
              <ol>
                <li>Ordered list item 1</li>
                <li>Ordered list item 2</li>
                <li>Ordered list item 3</li>
              </ol>
            </el-space>
          </el-space>
        </el-card>
      </div>

      <!-- Sidebar Example -->
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

.main-content {
  flex: 1;
  max-width: 900px;

  .el-card {
    margin-bottom: 1rem;
  }
}

.sidebar-example {
  width: 300px;
  position: sticky;
  top: 1rem;
  height: fit-content;
}

.mt-4 {
  margin-top: 1rem;
}

.context-menu-area {
  cursor: context-menu;
  padding: 2rem;
  text-align: center;
}

.context-menu-trigger {
  opacity: 0;
  pointer-events: none;
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
