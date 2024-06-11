<template>
  <div class="home">
    <TestContainer />
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import TestContainer from '@/components/TestContainer.vue';

@Options({
  components: {
    TestContainer,
  },
})
export default class HomeView extends Vue {}
</script>

<style lang="stylus" scoped>
.home {
  margin: 20px 0;
}
</style>
