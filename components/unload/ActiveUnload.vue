<template>
  <div class="block">
    <div class="block-header">
      <div class="block-id">{{unload.id}}</div>
      <div class="block-event text-bold" v-html="unload.event"/>
      <div class="block-close"/>
    </div>
    <div class="block-content">
      <div class="block-notice">
        Если после клика на ссылку загрузка не пошла, проверьте, не блокирует ли браузер скачивание архива
      </div>
      <div class="block-title text-bold">Ссылка для скачивания архива Выгрузки (.zip):</div>
      <div class="block-download">
        <a href="#">{{unload.download_link}}</a> <div  class="text-bold span-link" @click="copyLink(unload.download_link)">Скопировать ссылку</div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import {Unload} from "#build/types/types";
interface Props {
  unload: Unload,
}
defineProps<Props>()
const copyLink = async (link:string) => {
  try {
    await navigator.clipboard.writeText(link);
    alert('Ссылка скопирована');
  } catch(e) {
    alert('Произошла ошибка');
  }
}
</script>

<style lang="scss" scoped>
%close-span {
  content: "";
  display: block;
  background-color: #fff;
  width: 1px;
  height: 25px;
  position: absolute;
}
.block {
  &-header {
    display: flex;
    align-items: center;
    background-color: #F5F5F9;
    justify-content:space-between;
  }
  &-close {
    background-color: #CD5C5C;
    position: relative;
    height: 50px;
    width: 50px;
    display: flex;
    justify-content: center;
    align-items: center;

    &::after {
      @extend %close-span;

      transform: rotate(-45deg);
    }
    &::before {
      @extend %close-span;
      transform: rotate(45deg);
    }
  }
  &-content {
    padding: 15px;
  }
  &-notice {
    margin-bottom: 8px;
    padding: 10px;
    text-align: center;
    background-color: #ECEFF3;
    border-radius: 5px;
  }
  &-id {
    padding: 15px 10px;
    background-color: #5F5EA0;
    font-size: 16px;
    margin-right: 10px;
    color: #fff;
  }
  &-event {
    flex: 0 1 80%;
  }
  a {
    margin-right: 10px;
    color: #9875D1;
  }
  &-download {
    margin-top: 5px;
  }
}
.span-link {
  cursor: pointer;
  text-decoration-line: underline;
  text-decoration-style: dashed;
  text-underline-offset: 3px;
}
</style>