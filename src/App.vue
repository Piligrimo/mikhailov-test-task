<script setup lang="ts">
import { ref } from 'vue';
import initialUserItems from './data/user-items.json'
import initialItemsToSelect from './data/items-to-select.json'

type Item = {
  id: number
  name: string
}
  
const userItems = ref(initialUserItems)
const itemsToSelect = ref(initialItemsToSelect)

const selectedUserItems = ref([])
const selectedItem = ref<Item | null>(null)

const selectItem = (item : Item) => {
  selectedItem.value = item
}
const selectUserItem = (item: Item) => {
  if (!selectedUserItems.value.find(({ id } : Item) => id === item.id))
    selectedUserItems.value.push(item)
}
const unselectUserItem = (item: Item) => {
  selectedUserItems.value = selectedUserItems.value.filter(({ id } : Item) => id !== item.id)
}

</script>

<template>
  <div>
    <section class="selection">
      <div class="selection__box _bordered">
        <ul class="lists__list" v-if="selectedUserItems.length">
          <li
            v-for="item in selectedUserItems"
            :key="item.id" 
            class="lists__list-item _bordered"
            @click="unselectUserItem(item)"
          >
            {{ item.name }}
          </li>
        </ul>
        <p v-else> Nothing is selected </p>
        <p> Selected: {{ selectedUserItems.length }}/{{ userItems.length }} </p>
      </div>
      <div class="selection__box _bordered">
        <p v-if="selectedItem"> {{ selectedItem.name }}</p>
        <p v-else> Nothing is selected </p>
      </div>
    </section>
    <section class="lists">
      <ul class="lists__list _bordered">
        <li  
          v-for="item in userItems" 
          :key="item.id"
          class="lists__list-item _bordered"
          @click="selectUserItem(item)"
        >
          {{ item.name }}
        </li>
      </ul>
      <ul class="lists__list _bordered">
        <li 
          v-for="item in itemsToSelect" 
          :key="item.id" 
          class="lists__list-item _bordered"
          @click="selectItem(item)"
        >
          {{ item.name }}
        </li>
      </ul>
    </section>
  </div>
</template>

<style scoped>

.lists {
  display: flex;
  flex-direction: row;
  align-items: start;
  justify-content: space-between  ;
  gap: 10px;
  width: 100%;
}
.lists__list {
  list-style-type: none;
  width: 100%;
  padding: 10px;
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
}
.lists__list-item  {
  padding: 5px;
  white-space: nowrap;
  cursor: pointer;
}

.selection {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}

.selection__box {
  padding: 10px;
  width: 300px;
}

._bordered {
  border: 3px solid #000;
}

</style>
