<template>
  <div class="testContainer grid__block-inner">
    <div class="testContainer__filters">
      <TestFilters
        :search-value="search"
        @toggle-search-input="toggleSearchInput"
        @toggle-search="toggleSearch"
      />
    </div>

    <div class="testContainer__tableBox">
      <TestTable :results="slicedResult" :sort="sort" @toggle-sort="toggleSort" />
    </div>

    <div class="testContainer__pagginationBox">
      <TestPaggination
        :results="slicedResult"
        :page-size="page"
        :pages-count="pagesCount"
        @toggle-prev="togglePrev"
        @toggle-next="toggleNext"
        @set-page="setPage"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { useRouter, useRoute } from 'vue-router';
import { defineComponent, computed, ref } from 'vue';

import TestFilters from './TestFilters.vue';
import TestTable from './TestTable.vue';
import TestPaggination from './TestPaggination.vue';

import data from '../utils/api';

export default defineComponent({
  name: 'TestContainer',
  components: {
    TestFilters,
    TestTable,
    TestPaggination,
  },

  setup() {
    const router = useRouter();
    const route = useRoute();

    const resultCopy = ref(data.results.slice());

    const results = ref(data.results);

    const options: any = {
      day: 'numeric',
      month: 'numeric',
      year: 'numeric',
    };

    enum SortVariables { // eslint-disable-line
      none = 'unactive', // eslint-disable-line
      top = 'top', // eslint-disable-line
      bottom = 'bottom' // eslint-disable-line
    }

    const page = ref<number>(1);
    const search = ref<string>('');
    const sort = ref([
      SortVariables.none,
      SortVariables.none,
      SortVariables.none,
      SortVariables.none,
      SortVariables.none,
      SortVariables.none,
    ]);

    const pagesCount = computed<number>(() => (Math.ceil(results.value.length / 20)));

    const slicedResult = computed(
      () => results.value?.slice((page.value - 1) * 20, page.value * 20),
    );

    const updateDateFormat = (str: string) => {
      const date = new Date(str);

      return date.toLocaleString('ru', options);
    };

    const togglePrev = (pageSize: number) => {
      page.value = pageSize;
      router.push({ query: { ...route.query, page: page.value } });
    };
    const toggleNext = (pageSize: number) => {
      page.value = pageSize;
      router.push({ query: { ...route.query, page: page.value } });
    };
    const setPage = (pageSize: number) => {
      page.value = pageSize;
      router.push({ query: { ...route.query, page: page.value } });
    };

    const toggleSearchInput = (searchValue: string) => {
      search.value = searchValue;
      router.push({ query: { ...route.query, search: search.value } });
    };

    const toggleSort = (index: number, isClick: boolean) => {
      for (let i = 0; i < sort.value.length; i += 1) {
        if (i !== index) {
          sort.value[i] = SortVariables.none;
        }
      }

      if (isClick) {
        if (sort.value[index] === SortVariables.none) {
          sort.value[index] = SortVariables.bottom;

          router.push(
            { query: { ...route.query, sortIndex: index, sortField: SortVariables.bottom } },
          );
        } else if (sort.value[index] === SortVariables.bottom) {
          sort.value[index] = SortVariables.top;

          router.push(
            { query: { ...route.query, sortIndex: index, sortField: SortVariables.top } },
          );
        } else {
          sort.value[index] = SortVariables.none;

          router.push(
            { query: { ...route.query, sortIndex: index, sortField: SortVariables.none } },
          );
        }
      }

      if (sort.value[index] === SortVariables.none) {
        results.value = resultCopy.value.slice();
      } else if (sort.value[index] === SortVariables.bottom) {
        results.value = results.value.sort(
          (prev: any, next: any) => {
            if (index === 0) {
              return prev.name.first.localeCompare(next.name.first);
            }
            if (index === 1) {
              return prev.gender.localeCompare(next.gender);
            }
            if (index === 2) {
              return prev.location.country.localeCompare(next.location.country);
            }
            if (index === 3) {
              return +new Date(prev.dob.date) - +new Date(next.dob.date);
            }
            if (index === 4) {
              return prev.email.localeCompare(next.email);
            }
            return prev.phone.localeCompare(next.phone);
          },
        );
      } else {
        results.value = results.value.sort(
          (prev: any, next: any) => {
            if (index === 0) {
              return next.name.first.localeCompare(prev.name.first);
            }
            if (index === 1) {
              return next.gender.localeCompare(prev.gender);
            }
            if (index === 2) {
              return next.location.country.localeCompare(prev.location.country);
            }
            if (index === 3) {
              return +new Date(next.dob.date) - +new Date(prev.dob.date);
            }
            if (index === 4) {
              return next.email.localeCompare(prev.email);
            }
            return next.phone.localeCompare(prev.phone);
          },
        );
      }

      page.value = 1;
    };

    const toggleSearch = () => {
      page.value = 1;

      results.value = data.results.filter((item: any) => item.name.title.includes(search.value)
        || item.name.first.includes(search.value)
        || item.name.last.includes(search.value)
        || item.gender.includes(search.value)
        || item.location.country.includes(search.value)
        || updateDateFormat(item.dob.date).includes(search.value)
        || item.email.includes(search.value)
        || item.phone.includes(search.value));

      resultCopy.value = results.value.slice();
    };

    if (route.query.search) {
      search.value = String(route.query.search);
      toggleSearch();
    }
    if (route.query.page) {
      page.value = Number(route.query.page);
    }
    if (route.query.sortIndex && route.query.sortField) {
      if (SortVariables.none === route.query.sortField) {
        sort.value[Number(route.query.sortIndex)] = SortVariables.none;
      } else if (SortVariables.bottom === route.query.sortField) {
        sort.value[Number(route.query.sortIndex)] = SortVariables.bottom;
      } else {
        sort.value[Number(route.query.sortIndex)] = SortVariables.top;
      }

      toggleSort(+route.query.sortIndex, false);
    }

    return {
      slicedResult,
      search,
      page,
      pagesCount,
      sort,
      setPage,
      togglePrev,
      toggleNext,
      toggleSearchInput,
      toggleSearch,
      toggleSort,
    };
  },
});
</script>

<style lang="stylus" scoped>
.testContainer  {
  width: 100%;
  position relative;
  &__filters {
    margin-bottom 17px;
  }
  &__tableBox {
    max-width: 100%;
  }
}
</style>
