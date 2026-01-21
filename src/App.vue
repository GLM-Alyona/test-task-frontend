<template>
  <div class="app">
    <h1>Тестовое задание VueJS</h1>
    
    <!-- Верхние блоки -->
    <div class="top-section">
      <!-- Слева: выбранные вещи пользователя -->
      <div class="top-block top-left">
        <h2>Выбранные вещи пользователя</h2>
        <div class="selected-items">
          <div 
            v-for="item in selectedUserItems" 
            :key="item.id"
            class="item-card"
          >
            {{ item.name }}
          </div>
          <div v-if="selectedUserItems.length === 0" class="empty-message">
            Выберите от 1 до 6 вещей снизу слева
          </div>
        </div>
      </div>
      
      <!-- Справа: выбранная вещь на выбор -->
      <div class="top-block top-right">
        <h2>Выбранная вещь на выбор</h2>
        <div class="selected-items">
          <div v-if="selectedChoiceItem" class="item-card">
            {{ selectedChoiceItem.name }}
          </div>
          <div v-else class="empty-message">
            Выберите 1 вещь снизу справа
          </div>
        </div>
      </div>
    </div>
    
    <!-- Нижние блоки -->
    <div class="bottom-section">
      <!-- Слева: вещи у пользователя -->
      <div class="bottom-block bottom-left">
        <h2>Вещи у пользователя</h2>
        <div class="items-grid">
          <div 
            v-for="item in userItems" 
            :key="item.id"
            class="item-card"
            :class="{ selected: isUserItemSelected(item.id) }"
            @click="toggleUserItem(item)"
          >
            {{ item.name }}
          </div>
        </div>
        <div class="selection-info">
          Выбрано: {{ selectedUserItems.length }} / 6
        </div>
      </div>
      
      <!-- Справа: вещи на выбор -->
      <div class="bottom-block bottom-right">
        <h2>Вещи на выбор</h2>
        <div class="items-grid">
          <div 
            v-for="item in choiceItems" 
            :key="item.id"
            class="item-card"
            :class="{ selected: selectedChoiceItem?.id === item.id }"
            @click="selectChoiceItem(item)"
          >
            {{ item.name }}
          </div>
        </div>
        <div class="selection-info">
          Можно выбрать только 1 вещь
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import './App.css'

export default {
  name: 'App',
  data() {
    return {
      userItems: [
        { id: 1, name: "Shoes 1" },
        { id: 2, name: "Shoes 2" },
        { id: 3, name: "Shoes 3" },
        { id: 4, name: "Shoes 4" },
        { id: 5, name: "T-shirt 1" },
        { id: 6, name: "T-shirt 2" },
        { id: 7, name: "T-shirt 3" },
        { id: 8, name: "T-shirt 4" }
      ],
      choiceItems: [
        { id: 11, name: "Jacket 1" },
        { id: 12, name: "Jacket 2" },
        { id: 13, name: "Jacket 3" },
        { id: 14, name: "Jacket 4" },
        { id: 15, name: "Hoodie 1" },
        { id: 16, name: "Hoodie 2" },
        { id: 17, name: "Hoodie 3" },
        { id: 18, name: "Hoodie 4" }
      ],
      selectedUserItemIds: [],
      selectedChoiceItem: null
    }
  },
  computed: {
    selectedUserItems() {
      // Возвращаем выбранные элементы в порядке выбора
      return this.selectedUserItemIds
        .map(id => this.userItems.find(item => item.id === id))
        .filter(item => item !== undefined)
    }
  },
  methods: {
    toggleUserItem(item) {
      const index = this.selectedUserItemIds.indexOf(item.id)
      
      if (index > -1) {
        // Убираем из выбранных
        this.selectedUserItemIds.splice(index, 1)
      } else {
        // Добавляем, если не достигнут лимит в 6 элементов
        if (this.selectedUserItemIds.length < 6) {
          this.selectedUserItemIds.push(item.id)
        } else {
          alert('Можно выбрать максимум 6 вещей')
        }
      }
    },
    isUserItemSelected(id) {
      return this.selectedUserItemIds.includes(id)
    },
    selectChoiceItem(item) {
      // Можно выбрать только 1 вещь, поэтому просто заменяем
      this.selectedChoiceItem = item
    }
  }
}
</script>

