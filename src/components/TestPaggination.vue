<template>
  <div class="testPaggination">
    <div
      v-if="pageSize !== 1"
      @click="togglePrev"
      @keydown="togglePrev"
      class="testPaggination__button"
    >
      Previous
    </div>

    <div
      v-for="page in pagesCount"
      :key="page"
      @click="setPage(page)"
      @keydown="setPage(page)"
      :class="['testPaggination__button', { testPaggination__button_active: page === pageSize }]"
    >
      {{ page }}
    </div>

    <div
      v-if="pageSize !== pagesCount && pagesCount !== 0"
      @click="toggleNext"
      @keydown="toggleNext"
      class="testPaggination__button"
    >
      Next
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'TestPaggination',

  emits: ['togglePrev', 'toggleNext', 'setPage'],

  props: {
    results: Array,
    pageSize: Number,
    pagesCount: Number,
  },
  setup(props, { emit }) {
    const togglePrev = () => emit('togglePrev', (props.pageSize || 1) - 1);
    const toggleNext = () => emit('toggleNext', (props.pageSize || 1) + 1);
    const setPage = (page: number) => emit('setPage', page);

    return {
      togglePrev,
      toggleNext,
      setPage,
    };
  },
});
</script>

<style lang="stylus" scoped>
.testPaggination {
  display: flex;
  justify-content: center;
  gap: 10px;

  &__button {
    text-decoration: underline;
    cursor: pointer;
    color: blue;

    &:hover {
      text-decoration: none;
      opacity: .4;
    }

    &_active {
      text-decoration: none;
      cursor: default;
      color: black;
      font-weight: bold;
      &:hover {
        opacity: 1;
      }
    }
  }
}
</style>
