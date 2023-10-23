<template>
  <div ref="unload" class="block" @click="$emit('activeUnload', unload.id)">
    <p v-html="unload.task_date" />

    <p>Статус задачи: <span class="text-bold">{{unload.status_text}}</span></p>
    <p>ID выгрузки: <span class="text-bold">{{unload.id}}</span></p>
    <p v-html="unload.event" />
    <p>Выгружено заказов: <span class="text-bold">6 из 6</span></p>
    <p>Размер выгрузки: <span class="text-bold">{{unload.size}}</span></p>
  </div>
</template>

<script setup lang="ts">
import {onActivated} from "vue";
import {Unload} from "#build/types/types";
interface Props {
  unload: Unload,
}
defineEmits(['activeUnload']);
const unloadCard:HTMLDivElement | null = ref(null);
const props = defineProps<Props>()

onActivated(() => {
  let status:string = '';
  switch(props.unload.status) {
    case 'green':
      status = '--color_success';
      break;
    default:
      status = '--color_danger';
      break;
  }
  if(unloadCard.value !== null) {
    unloadCard.style.borderLeft = `10px solid ${status}`;
  }

})

</script>

<style lang="scss" scoped>
.block {
  padding: 15px;
  border-left: 10px solid var(--color_success);
  border-radius: 3px;
  font-size: 14px;
  cursor: pointer;
}
</style>