<template>
  <button
    @click="handleClick"
    :class="[
      'px-4 py-4 text-light bg-primary rounded hover:bg-danger text-center',
      btnclass,
    ]"
  >
    <slot></slot>
  </button>
</template>

<script setup>
import { defineProps } from "vue";

const props = defineProps({
  onClick: {
    type: Function,
    default: () => {},
  },
  btnclass: {
    type: String,
    default: "",
  },
});

const handleClick = () => {
  props.onClick();
};
</script>
