<template>
  <section id="dashboard">
    <div class="grid">
      <div class="grid--column">
        <div class="grid--cell top-left">
          <router-view name="top-left"></router-view>
        </div>
        <div class="grid--cell bottom-left">
          <router-view name="bottom-left"></router-view>
        </div>
      </div>
      <div class="grid--column">
        <div class="grid--cell top-right">
          <router-view name="top-right"></router-view>
        </div>
        <div class="grid--cell bottom-right">
          <router-view name="bottom-right"></router-view>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
};
</script>

<style lang="scss">
#dashboard {
  display: flex;
  flex-direction: row;
  flex: 1;

  .grid {
    display: flex;
    flex-direction: row;
    flex: 1;
    padding: $space;
  }

  .grid--column {
    display: flex;
    flex-direction: column;
    flex: 1 0 50%;

    &:first-child {
      max-width: toRem(570px);
    }
  }

  .grid--cell {
    display: flex;
    flex-direction: row;
    flex: 1;
    padding: $space;

    > * {
      flex: 1;
    }
  }
}
</style>

