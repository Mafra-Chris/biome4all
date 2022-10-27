<script setup lang="ts">
const props = defineProps({
  level: { type: Number, required: true },
  name: { type: String, required: true },
});

function setLabel() {
  switch (props.level) {
    case 1:
      return 'Muito Baixo';

    case 2:
      return 'Baixo';

    case 3:
      return 'Médio';

    case 4:
      return 'Alto';

    case 5:
      return 'Muito Alto';

    default:
      break;
  }
}
onMounted(() => {
  setLabel();
});
</script>
<template>
  <h1 class="text-left">{{ name }}</h1>
  <div class="grid grid-cols-6 gap-x-1 items-center">
    <div
      :class="`${
        props.level >= 1 ? 'bg-orange-900' : 'bg-gray-400'
      }  h-2 rounded-l-full`"
    ></div>
    <div
      :class="`${props.level >= 2 ? 'bg-orange-500' : 'bg-gray-400'}  h-2`"
    ></div>
    <div
      :class="`${props.level >= 3 ? 'bg-amber-400' : 'bg-gray-400'}  h-2`"
    ></div>
    <div
      :class="`${props.level >= 4 ? 'bg-green-400' : 'bg-gray-400'}  h-2`"
    ></div>
    <div
      :class="`${
        props.level >= 5 ? 'bg-green-600' : 'bg-gray-400'
      }  h-2 rounded-r-full`"
    ></div>
    <div class="text-left">{{ setLabel() }}</div>
  </div>
</template>
