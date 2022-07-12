<template>

  <div class="card">
    <img :src="model.link" alt="img">
    <span>{{ model.name }}</span>
    <p>{{ model.description }}</p>
    <h2>{{ model.price }} руб.</h2>
    <img @click="removeProd" class="img-delete" :src="require('/src/image/trash.svg')" alt="img">
  </div>
</template>

<script>
export default {
  name: "ProductItem",
  props: {
    model: {
      name: String,
      description: String,
      link: String,
      price: Number
    }
  },
  setup(props, {emit}) {
    const removeProd = () => {
      emit('removeProd')
    }

    return {
      removeProd
    }
  }
}

</script>

<script>

</script>

<style scoped>
div {
  width: 330px;
  height: 423px;
  display: flex;
  position: relative;
  flex-direction: column;
  background: #FFFEFB;
  box-shadow: 0 20px 30px rgba(0, 0, 0, 0.04), 0 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
  margin: 8px 8px 8px 8px;

}

img {
  height: 200px;
  border-radius: 4px 4px 0 0;
  object-fit: cover;
}

span {
  font-weight: 600;
  font-size: 20px;
  padding: 16px 0 16px 16px;
}

p {
  font-weight: 400;
  font-size: 16px;
  height: 80px;
  padding: 0 16px 0 16px;
  margin: 0;
  text-overflow: ellipsis;
  overflow: hidden;
}


h2 {
  font-weight: 600;
  font-size: 24px;
  padding: 32px 0 24px 16px;
  margin: 0;
}

.img-delete {
  transition: 0.3s opacity;
  opacity: 0;
  position: absolute;
  width: 32px;
  height: 32px;
  right: -11px;
  top: -8px;
}


.card:hover .img-delete {
  transition: 0.3s opacity;
  opacity: 1;
}


</style>
