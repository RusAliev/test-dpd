<template>
  <div class="testFilters">
    <label for="search" class="testFilters__search">
      <span class="grid__block-name">Поиск</span>

      <div class="testFilters__inputBox">
        <input
          v-model="search"
          @input="toggleSearchInput"
          @change="toggleSearch"
          type="text"
          id="search"
          class="testFilters__searchInput"
        >

        <button @click="toggleSearch">Поиск</button>
      </div>
    </label>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'TestFilters',

  emits: ['toggleSearchInput', 'toggleSearch'],

  props: {
    searchValue: {
      type: String,
      default: '',
    },
  },
  setup(props, { emit }) {
    const search = ref<string>(String(props.searchValue));

    const toggleSearchInput = () => emit('toggleSearchInput', search.value);
    const toggleSearch = () => emit('toggleSearch');

    return {
      search,
      toggleSearchInput,
      toggleSearch,
    };
  },
});
</script>

<style lang="stylus" scoped>
.testFilters {
  &__search {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    margin-left: 10px;
  }

  &__inputBox {
    width: 100%;
    display: flex;
    justify-content: flex-start;
    gap: 15px;
  }

  &__searchInput {
    padding: 10px 20px;
    min-width: 30%;
    border: 2px solid #ededf5;
    font-size: 14px;
    font-weight: 600;
  }
}
</style>
