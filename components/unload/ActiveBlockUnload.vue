<template>
  <div class="block">
    <div class="block-title text-bold">Выгрузка</div>
    <div class="block-content">
      <slot/>
    </div>
  </div>
</template>

<script>

</script>

<style lang="scss" scoped>
  .block {
    padding: 15px;
    &-content {
      margin-top: 5px;
    }

  }
  @include start-at(xslg) {
    .block {
      &-title {
        font-size: 20px;
      }
    }
  }
  @include active-by(xslg) {
    .block {
      &-title {
        font-size: 18px;
      }
    }
  }
</style>