<script setup>
import { ref } from "vue";
import BaseCard from "./components/BaseCard.vue";

const memes = ref([]);
let allMemes = [];

const loadData = async () => {
  const response = await fetch("https://api.imgflip.com/get_memes");
  const { data } = await response.json();

  console.log(data.memes);
  memes.value = data.memes;
  allMemes = data.memes;
};

const searchMeme = (event) => {
  const value = event.target.value;
  memes.value = allMemes.filter((meme) =>
    meme.name.toLowerCase().includes(value.toLowerCase())
  );
};

loadData();
</script>

<template>
  <h1>Meme Search</h1>

  <input
    class="input input-bordered w-full max-w-xs"
    type="text"
    placeholder="Buscar meme"
    @input="searchMeme"
  />

  <section class="container">
    <BaseCard v-for="meme in memes" :key="meme.id" :meme="meme" />
  </section>
</template>

<style scoped>
.base-input {
  padding: 5px 8px;
}

.container {
  display: flex;
  flex-wrap: wrap;
}

.card {
  display: flex;
  flex-direction: column;
  border: 1px solid lightblue;
  border-radius: 8px;
  margin: 5px;
  position: relative;
  max-width: 180px;
  padding: 10px;
  align-items: center;
}

.card .icon {
  width: 30px;
  color: black;
  position: absolute;
  right: 15px;
  cursor: pointer;
}

.card img {
  width: 180px;
}
</style>
