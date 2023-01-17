<script setup>
import { computed } from 'vue';

const props = defineProps({
  col: Number,
  content: {
    type: String,
    required: true,
  },
  id: Number,
  row: Number,
  safe: Boolean,
  selected: {
    type: Boolean,
    required: true,
  },
});
const emit = defineEmits(["select"]);

const computedClass = computed(() => {
  if (props.selected) return "selected";
  if (props.safe) return "safe";
  return "";
});

function click() {
  emit("select", { row: props.row, col: props.col });
}
</script>

<template>
  <div class="cell" @click="click" :class="computedClass">
    <h3>
      {{ content }}
    </h3>
  </div>
</template>

<style scoped>
div {
  width: 30px;
  height: 30px;
  cursor: pointer;
  background-color: white;
}

div.selected {
  background-color: red;
}

div.safe {
  background-color: green;
}

h3 {
  font-size: 1.2rem;
  text-align: center;
}

.cell h3 {
  text-align: center;
  width: 30px;
}
</style>
