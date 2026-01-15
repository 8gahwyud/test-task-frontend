<template>
  <div id="app">
    <div class="container">
      <!-- Верхняя часть: выбранные вещи -->
      <div class="top-section">
        <div class="selected-box user-selected">
          <div class="box-header">
            <h2>Выбранные вещи пользователя</h2>
            <span class="counter">{{ selectedUserItems.length }}/6</span>
          </div>
          <div class="items-grid selected-grid">
            <div 
              v-for="item in selectedUserItems" 
              :key="item.id"
              class="item-card selected"
            >
              {{ item.name }}
            </div>
            <div v-if="selectedUserItems.length === 0" class="empty-state">
              Выберите вещи из списка ниже
            </div>
          </div>
        </div>
        
        <div class="selected-box choice-selected">
          <div class="box-header">
            <h2>Выбранные вещи на обмен</h2>
            <span class="counter">{{ selectedChoiceItems.length }}/{{ selectedUserItems.length || 0 }}</span>
          </div>
          <div class="items-grid selected-grid">
            <div 
              v-for="item in selectedChoiceItems" 
              :key="item.id"
              class="item-card selected"
            >
              {{ item.name }}
            </div>
            <div v-if="selectedChoiceItems.length === 0" class="empty-state">
              Выберите вещи из списка справа
            </div>
          </div>
        </div>
      </div>

      <!-- Нижняя часть: доступные вещи -->
      <div class="bottom-section">
        <div class="items-box user-items">
          <div class="box-header">
            <h2>Вещи пользователя</h2>
            <span class="hint">Выберите от 1 до 6 вещей</span>
          </div>
          <div class="items-grid">
            <div 
              v-for="item in userItems" 
              :key="item.id"
              class="item-card clickable"
              :class="{ active: isUserItemSelected(item.id) }"
              @click="toggleUserItem(item)"
            >
              {{ item.name }}
            </div>
          </div>
        </div>
        
        <div class="items-box choice-items">
          <div class="box-header">
            <h2>Вещи на выбор</h2>
            <span class="hint">Выберите столько же, сколько отдаете</span>
          </div>
          <div class="items-grid">
            <div 
              v-for="item in choiceItems" 
              :key="item.id"
              class="item-card clickable"
              :class="{ active: isChoiceItemSelected(item.id) }"
              @click="toggleChoiceItem(item)"
            >
              {{ item.name }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'App',
  setup() {
    const userItems = ref([
      { id: 1, name: "Кроссовки 1" },
      { id: 2, name: "Кроссовки 2" },
      { id: 3, name: "Кроссовки 3" },
      { id: 4, name: "Кроссовки 4" },
      { id: 5, name: "Футболка 1" },
      { id: 6, name: "Футболка 2" },
      { id: 7, name: "Футболка 3" },
      { id: 8, name: "Футболка 4" }
    ])

    const choiceItems = ref([
      { id: 11, name: "Куртка 1" },
      { id: 12, name: "Куртка 2" },
      { id: 13, name: "Куртка 3" },
      { id: 14, name: "Куртка 4" },
      { id: 15, name: "Худи 1" },
      { id: 16, name: "Худи 2" },
      { id: 17, name: "Худи 3" },
      { id: 18, name: "Худи 4" }
    ])

    const selectedUserItems = ref([])
    const selectedChoiceItems = ref([])

    const toggleUserItem = (item) => {
      const index = selectedUserItems.value.findIndex(i => i.id === item.id)
      
      if (index > -1) {
        selectedUserItems.value.splice(index, 1)
        
        if (selectedChoiceItems.value.length > selectedUserItems.value.length) {
          selectedChoiceItems.value.pop()
        }
      } else {
        if (selectedUserItems.value.length < 6) {
          selectedUserItems.value.push(item)
        }
      }
    }

    const toggleChoiceItem = (item) => {
      const index = selectedChoiceItems.value.findIndex(i => i.id === item.id)
      
      if (index > -1) {
        selectedChoiceItems.value.splice(index, 1)
      } else {
        if (selectedChoiceItems.value.length < selectedUserItems.value.length) {
          selectedChoiceItems.value.push(item)
        }
      }
    }

    const isUserItemSelected = (id) => {
      return selectedUserItems.value.some(item => item.id === id)
    }

    const isChoiceItemSelected = (id) => {
      return selectedChoiceItems.value.some(item => item.id === id)
    }

    return {
      userItems,
      choiceItems,
      selectedUserItems,
      selectedChoiceItems,
      toggleUserItem,
      toggleChoiceItem,
      isUserItemSelected,
      isChoiceItemSelected
    }
  }
}
</script>

<style scoped>
#app {
  min-height: 100vh;
  background: #f5f5f5;
  padding: 30px 20px;
}

.container {
  max-width: 1400px;
  margin: 0 auto;
}

h2 {
  font-size: 16px;
  margin: 0;
  color: #333;
  font-weight: 600;
}

.top-section,
.bottom-section {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 20px;
}

.selected-box,
.items-box {
  background: white;
  border: 1px solid #ddd;
  padding: 20px;
}

.box-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
  padding-bottom: 10px;
  border-bottom: 1px solid #eee;
}

.counter {
  background: #333;
  color: white;
  padding: 4px 12px;
  font-size: 13px;
  font-weight: 600;
}

.hint {
  color: #999;
  font-size: 12px;
}

.items-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
  gap: 10px;
}

.selected-grid {
  min-height: 100px;
}

.item-card {
  padding: 15px;
  border: 2px solid #ddd;
  background: #fafafa;
  text-align: center;
  transition: all 0.2s ease;
  font-size: 14px;
}

.item-card.clickable {
  cursor: pointer;
}

.item-card.clickable:hover {
  border-color: #666;
  background: white;
}

.item-card.active {
  border-color: #4CAF50;
  background: #e8f5e9;
  font-weight: 600;
}

.item-card.selected {
  border-color: #2196F3;
  background: #e3f2fd;
  font-weight: 600;
}

.empty-state {
  grid-column: 1 / -1;
  text-align: center;
  padding: 30px 20px;
  color: #999;
  font-size: 14px;
}

@media (max-width: 768px) {
  .top-section,
  .bottom-section {
    grid-template-columns: 1fr;
  }
}
</style>
