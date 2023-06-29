<template>
  <div>
    <table-base :items="items" :fields="fields" :order-by="orderBy" @orderChanged="orderChanged" >
      <template #sortIconAsc>
        <small>
          (active asc)
        </small>
      </template>
      <template #sortIconDesc>
        <small>
          (active desc)
        </small>
      </template>
      <template #sortIconClassNotActive>
        <small>
          (orderable)
        </small>
      </template>
      <template #thInner="data">
        <span v-if="data.field.tooltip !== undefined" :title="data.field.tooltip">
          {{ data.field.label }} (with tooltip)
        </span>
        <span v-else>
          {{ data.field.label }}
        </span>
      </template>
      <template #td="data">
        <div v-if="data.field === 'price'">
          {{ data.item }} + Using another column value: {{ items[data.index].name }}
        </div>
        <div v-else>
          {{ data.item }}
        </div>
      </template>
  </table-base>

    Order:
    {{ orderBy }} {{ !sortDesc ? 'ASC' : 'DESC' }}
  </div>
</template>

<script>

import {defineComponent} from "vue";
import TableBase from "@/components/table-base/TableBase.vue";

export default defineComponent({
  components: {
    TableBase,
  },
  data() {
    return {
      fields: [
        {
          key: 'id',
          label: 'ID',
        },
        {
          key: 'name',
          label: 'Name',
          tooltip: 'Tooltip Test',
        },
        {
          key: 'price',
          label: 'Price',
          sort: false,
        },
      ],
      items: [
        {
          id: 1,
          name: 'Test1',
          price: 150,
          trClass: 'bg-danger',
        },
        {
          id: 2,
          name: 'Test2',
          price: 250,
          tdClass: {
            price: 'bg-success',
          }
        },
      ],
      orderBy: 'name',
      sortDesc: false,
    }
  },
  methods: {
    orderChanged({ sortDesc, orderBy }) {
      this.sortDesc = sortDesc
      this.orderBy = orderBy
    },
  }
})
</script>


<style>
.bg-danger {
  background-color: #da6c6c;
}
.bg-success {
  background-color: #2ba41e;
}
</style>
