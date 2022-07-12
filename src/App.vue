<script>
import ProductItem from "./components/ProductItem";
import FormProduct from "./components/FormProduct";
import HeaderProduct from "@/components/HeaderProduct";
import {onMounted, reactive} from "vue";

export default {
  components: {
    ProductItem,
    FormProduct,
    HeaderProduct
  },
  setup() {
    const state = reactive({
      products: [],
      isLoading:true
    })

    const addProduct = ({name, description, link, price}) => {
      state.products = [...state.products, {
        id: Math.floor(Math.random() * (1000 - 10) + 10),
        name,
        description,
        link,
        price
      }]

      saveProdInLocal()

    }

    const saveProdInLocal = () => {
      localStorage.setItem('arr', JSON.stringify(state.products))
    }

    const sortMin = () => {
      state.products = state.products.sort((a, b) => a.price > b.price ? 1 : -1)
    }

    const sortMax = () => {
      state.products = state.products.sort((a, b) => a.price < b.price ? 1 : -1)
    }

    const sortName = () => {
      state.products = state.products.sort((a, b) => a.name.toLowerCase() > b.name.toLowerCase() ? 1 : -1)
    }

    const sortDef = () => {
      return state.products
    }

    const removeProd = (id) => {
      state.products = state.products.filter(x => x.id !== id)
      saveProdInLocal()
    }


    onMounted(() => {
      if (localStorage.getItem('arr')) {
        state.products = (JSON.parse(localStorage.getItem('arr')))
      }
      setTimeout(() => {
        state.isLoading = false
      }, 1500)

    })

    return {
      state,
      addProduct,
      sortMin,
      sortMax,
      sortName,
      sortDef,
      removeProd
    }
  }
}


</script>

<template>
  <HeaderProduct
      @sortMin="sortMin"
      @sortMax="sortMax"
      @sortName="sortName"
      @sortDef="sortDef"
  />
  <div class="main">
    <div class="form">
      <FormProduct @onAddNewProduct="addProduct"/>
    </div>
    <div v-if="state.isLoading" class="lds-ripple"><div></div><div></div></div>
    <Transition>
      <div v-if="!state.isLoading" class="products">
        <TransitionGroup name="list">
          <ProductItem
              v-for="prod in state.products"
              :model="prod" :key="prod.id"
              @removeProd="removeProd(prod.id)"/>
        </TransitionGroup>
      </div>
    </Transition>

  </div>
</template>

<style>
html {
  background: #E5E5E5;
  font-family: 'Source Sans Pro', sans-serif;
}

body {
  margin: 0;
  padding: 0;
}

.main {
  padding: 16px 30px 0 32px;
  display: flex;
  flex-flow: row;
}

.form {

  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  width: fit-content;
  height: fit-content;
}


.products {
  display: flex;
  flex-wrap: wrap;
  width: fit-content;
  height: fit-content;
  margin: -8px 0 0 8px;
}


.list-move,
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.list-leave-active {
  position: absolute;
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}


.lds-ripple {
  display: block;
  margin: auto;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-ripple div {
  position: absolute;
  border: 4px solid #fff;
  opacity: 1;
  border-radius: 50%;
  animation: lds-ripple 1s cubic-bezier(0, 0.2, 0.8, 1) infinite;
}
.lds-ripple div:nth-child(2) {
  animation-delay: -0.5s;
}
@keyframes lds-ripple {
  0% {
    top: 36px;
    left: 36px;
    width: 0;
    height: 0;
    opacity: 0;
  }
  4.9% {
    top: 36px;
    left: 36px;
    width: 0;
    height: 0;
    opacity: 0;
  }
  5% {
    top: 36px;
    left: 36px;
    width: 0;
    height: 0;
    opacity: 1;
  }
  100% {
    top: 0;
    left: 0;
    width: 72px;
    height: 72px;
    opacity: 0;
  }
}

</style>
