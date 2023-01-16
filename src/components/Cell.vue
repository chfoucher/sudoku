<script setup>
import { reactive } from 'vue';
const props = defineProps({
  content: {
    type: String,
    required: true,
  },
  id: {
    type: Number,
    required: false,
  },
  selected: {
    type: Boolean,
    required: true,
  }
});
const emit = defineEmits(["select"]);

function click(event) {
    emit("select", props.id);
}
</script>

<template>
  <div class="cell" @click="click" :class="{ selected: selected }">
    <h3>
      {{ content }}
    </h3>
  </div>
</template>

<style scoped>
div {
    width: 30px;
    height: 30px;
}
h3 {
  font-size: 1.2rem;
}

.cell h3 {
  text-align: center;
  width: 30px;
}

div.selected {
    background-color: red;
}

@media (min-width: 1024px) {
  .cell h3 {
    text-align: left;
  }
}
</style>
