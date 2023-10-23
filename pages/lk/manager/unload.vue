<template>
<main class="unload">
  <div class="unload-block">
    <ActiveBlockUnload>
      <p class="text-bold">Выполняет работу:</p>
      <ul>
        <li>- Собирает фотографии из заказов пользователей.
        </li>
        <li>- Выгружает по папкам.
        </li>
      </ul>
    </ActiveBlockUnload>
  </div>
  <div class="unload-main">

    <ActiveUnload v-if="show" :unload="activeUnload" />
    <Notice v-else>
      Для того,чтобы посмотреть информацию о <span class="text-bold">Выгрузке</span>, а также её скачать, нажмите на требуюмую, выгрузку в стоблце слева
    </Notice>
  </div>
  <div class="unload-cards">
    <UnloadCard  v-for="(unload) in unloadsList" :key="unload.id" :unload="unload" @active-unload="getActiveUnload"/>
  </div>

</main>
</template>

<script lang="ts" setup>
import {onMounted,watch} from "vue";
import ActiveBlockUnload from "@/components/unload/ActiveBlockUnload.vue";
import UnloadCard from "@/components/unload/UnloadCard.vue";
import {useAPIFetch} from "@/composables/useAPIFetch";
import ActiveUnload from "@/components/unload/ActiveUnload.vue";
import {Unload} from "#build/types/types";

const unloadsList:Unload[] = reactive([]);
const activeUnload:Unload = ref({});
onMounted( () => {
  setTimeout(async () => {
    const {data} = await useAPIFetch('https://dev-cabinet.seenday.com/e.scripts', {
      query: {
        page: 'pages:unload',
        event: 'get'
      }
    })

    unloadsList.push(...JSON.parse(data.value).response.data);
  },200)
})
const show: boolean = ref(false);
const getActiveUnload = async (activeUnloadId:string) => {
  const {data} = await useAPIFetch('https://dev-cabinet.seenday.com/e.scripts', {
    query: {
      page: 'pages:unload',
      event: 'get',
      unload_id: activeUnloadId
    }
  }) // можно сделать loading загрузки данных
  if(JSON.parse(data.value).response.data.length) {
    activeUnload.value = JSON.parse(data.value).response.data[0];
  }
}
watch(() => activeUnload, () => {
  show.value = true // просто для примера. так же можно сделать проверку на закрытие
}, {deep:true})
</script>

<style lang="scss" scoped>
ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
.unload {
  font-size: 14px;
  line-height: 1.2;
  display: grid;
  gap:15px;
  &-block {
    ul {
      * {
        margin-top: 5px;
      }
    }
  }
  &-cards {
    .block:not(:last-child) {
      margin-bottom: 15px;
    }
  }
}
@include start-at(xslg) {
  .unload {
    grid-template-rows: auto 1fr;
    grid-template-columns: 1fr 1fr;
    &-cards {
      grid-column:1/2;
    }
    &-main {
      grid-column: 2/-2;
      grid-row: -3/-1;
    }
  }
}
@include active-by(xslg) {
  .unload {
    grid-template-rows: auto;
    grid-template-columns: 1fr;
    &-cards {
      grid-column:1/2;
    }
    &-main {
      grid-column: 1/2;
    }
  }
}
</style>