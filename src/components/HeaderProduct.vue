<template>
  <div class="main">
    <h1>Добавления товаров</h1>
    <select @change="$emit(state.value)" v-model="state.value">
      <option value="sortDef">По умолчанию</option>
      <option value="sortMin">Цена ↑</option>
      <option value="sortMax">Цена ↓</option>
      <option value="sortName">По названию</option>
    </select>
  </div>
</template>

<script>
import {reactive} from "vue";

export default {
  name: "HeaderProduct",

  setup() {
    const state = reactive({
      value: 'sortDef'
    })

    return {
      state
    }
  }
}
</script>

<style scoped>
.main {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  padding: 32px 36px 0 32px;
  height: fit-content;
}

button {
  width: 121px;
  height: 36px;
  align-self: center;
}

select {
  font-size: 12px;
  height: 36px;
  text-align: center;
  padding: 10px 16px;
  appearance: none;
  font-style: normal;
  font-weight: 400;
  line-height: 15px;
  color: #B4B4B4;
  background: #FFFEFB;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  border: none;
  outline: none;
}


h1 {
  font-size: 28px;
  margin: 0;
}
</style>
