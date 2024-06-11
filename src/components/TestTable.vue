<template>
  <div class="table-responsive container">
    <table class="commonTable">
      <thead>
        <th class="table__headCell">Аватар</th>

        <th class="table__headCell">
          ФИО
          <img
            class="table__sortIcon"
            :src="require(`@/assets/icons/arrow-${sort[0]}-icon.png`)"
            alt="sort icon"
            @click="toggleSort(0)"
            @keydown="toggleSort(0)"
          >
        </th>

        <th class="table__headCell">
          Пол
          <img
            class="table__sortIcon"
            :src="require(`@/assets/icons/arrow-${sort[1]}-icon.png`)"
            alt="sort icon"
            @click="toggleSort(1)"
            @keydown="toggleSort(1)"
          >
        </th>

        <th class="table__headCell">
          Страна
          <img
            class="table__sortIcon"
            :src="require(`@/assets/icons/arrow-${sort[2]}-icon.png`)"
            alt="sort icon"
            @click="toggleSort(2)"
            @keydown="toggleSort(2)"
          >
        </th>

        <th class="table__headCell">
          Дата рождения
          <img
            class="table__sortIcon"
            :src="require(`@/assets/icons/arrow-${sort[3]}-icon.png`)"
            alt="sort icon"
            @click="toggleSort(3)"
            @keydown="toggleSort(3)"
          >
        </th>

        <th class="table__headCell">
          Адрес электронной почты
          <img
            class="table__sortIcon"
            :src="require(`@/assets/icons/arrow-${sort[4]}-icon.png`)"
            alt="sort icon"
            @click="toggleSort(4)"
            @keydown="toggleSort(4)"
          >
        </th>

        <th class="table__headCell">
          Телефон
          <img
            class="table__sortIcon"
            :src="require(`@/assets/icons/arrow-${sort[5]}-icon.png`)"
            alt="sort icon"
            @click="toggleSort(5)"
            @keydown="toggleSort(5)"
          >
        </th>
      </thead>

      <tbody v-for="(person, index) in results" :key="index">
        <td class="table__tableCell">
          <img class="table__avatar" :src="person.picture.medium" alt="avatar">
        </td>
        <td class="table__tableCell">
          {{ `${person.name.title} ${person.name.first} ${person.name.last}` }}
        </td>
        <td class="table__tableCell">{{ person.gender }}</td>
        <td class="table__tableCell">{{ person.location.country }}</td>
        <td class="table__tableCell">{{ updateDateFormat(person.dob.date) }}</td>
        <td class="table__tableCell">{{ person.email }}</td>
        <td class="table__tableCell">{{ person.phone }}</td>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'TestTable',

  emits: ['toggleSort'],

  props: {
    results: Array,
    sort: {
      type: Array,
      default: () => [
        'unactive',
        'unactive',
        'unactive',
        'unactive',
        'unactive',
        'unactive',
      ],
    },
  },
  setup(_, { emit }) {
    const options: any = {
      day: 'numeric',
      month: 'numeric',
      year: 'numeric',
    };

    const updateDateFormat = (str: string) => {
      const date = new Date(str);

      return date.toLocaleString('ru', options);
    };

    const toggleSort = (index: number) => emit('toggleSort', index, true);

    return {
      updateDateFormat,
      toggleSort,
    };
  },
});
</script>

<style lang="stylus" scoped>
.commonTable {
  margin-bottom 15px
}
.container {
  margin-bottom: 20px;
  margin-right: 30px;
  th {
    white-space: nowrap;
  }
}
.blured {
  opacity: .4;
}

.table {
  &__headCell, &__tableCell {
    min-width: 50px;
    white-space: nowrap;
    text-align: left;
    padding: 5px 10px;
  }
  &__avatar {
    border: 3px solid #FBCE51;
    border-radius: 4px;
  }
  &__sortIcon {
    width: 15px;
    &:hover {
      cursor: pointer;
      opacity: .6;
    }
  }
}
</style>
