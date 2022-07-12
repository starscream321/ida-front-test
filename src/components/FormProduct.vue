<template>
  <form @submit.prevent="onAddNewProduct">
    <p>
      <label for="name">Название товара<span class="dot"></span></label>
      <input
          :class="{hasError : v$.name.$error}"
          placeholder="Введите наименование товара"
          id="name"
          v-model="v$.name.$model"
          type="text"
          name="name"
      >
      <span class="error" v-if="v$.name.$error">Поле является обязательным</span>
    </p>
    <p>
      <label for="description">Описание товара</label>
      <textarea
          placeholder="Введите описание товара"
          id="description"
          v-model="state.description"
          name="description"
      />
    </p>
    <p>
      <label for="link">Ссылка на изображение товара<span class="dot"></span></label>
      <input
          :class="{hasError : v$.link.$error}"
          placeholder="Введите ссылку"
          id="link"
          v-model="v$.link.$model"
          type="text"
          name="link"
      >
      <span class="error" v-if="v$.link.$error">Поле является обязательным</span>
    </p>
    <p>
      <label for="price">Цена товара<span class="dot"></span></label>
      <input
          :class="{hasError : v$.price.$error}"
          placeholder="Введите цену"
          id="price"
          v-model="v$.price.$model"
          type="text"
          name="price"
          onblur="this.value = this.value.replace(/[^\d]/g, '').replace(/\B(?=(?:\d{3})+(?!\d))/g, ' ')"
      >
      <span class="error" v-if="v$.price.$error">Поле является обязательным</span>
    </p>
    <p>
      <input
          :class="{ btn_invalid : !v$.$invalid}"
          class="button"
          type="submit"
          value="Добавить товар"
      >
    </p>
  </form>
</template>

<script>

import {computed, reactive} from "vue";
import useVuelidate from '@vuelidate/core'
import {minLength, required, url, numeric} from "@vuelidate/validators";


export default {
  name: "FormProduct",
  setup(props, {emit}) {


    const state = reactive({
      name: '',
      description: '',
      link: '',
      price: '',
      error: false
    })


    const rules = computed(() => {
      return {
        name: {required, minlength: minLength(1)},
        price: {required, numeric},
        link: {required, url: url},
      }
    })

    const v$ = useVuelidate(rules, state)


    const onAddNewProduct = async () => {
      const newNum = new Intl.NumberFormat('ru-RU').format(state.price)
      const result = await v$.value.$validate()
      if (result === true) {
        emit('onAddNewProduct', {name: state.name, description: state.description, price: newNum, link: state.link})
        state.name = ''
        state.description = ''
        state.link = ''
        state.price = ''
        v$.value.$reset()
      } else {
        state.error = true
      }

    }

    return {
      state,
      v$,
      onAddNewProduct
    }
  },
  methods: {}
}
</script>


<style scoped>
form {
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #FFFEFB;
  border-radius: 4px;
  padding: 24px;
  width: 332px;

}

p {
  display: flex;
  flex-direction: column;
}

input {
  width: 252px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  border: none;
  padding: 10px 16px;
  font-size: 12px;
  font-weight: 400;
  line-height: 15px;
  outline: inherit;
}


textarea {
  border: none;
  height: 108px;
  resize: none;
  width: 252px;
  outline: none;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  padding: 10px 16px;
  font-size: 13px;
  font-weight: 400;
  line-height: 15px;
  font-family: 'Source Sans Pro', sans-serif;

}

.button {
  border-radius: 10px;
  width: 284px;
  color: #B4B4B4;

}

.error {
  color: #FF8484;
  font-weight: 400;
  font-size: 8px;
  padding-top: 4px;
}

.hasError {
  border: 1px solid #FF8484;
}

label {
  width: fit-content;
  padding-bottom: 4px;
  font-size: 10px;
  font-weight: 400;
}

.dot:after {
  color: #FF8484;
  content: " *";
}

.btn_invalid {
  background-color: #7BAE73;
  color: #FFFFFF;
}

::placeholder {
  color: #B4B4B4;
}


</style>
