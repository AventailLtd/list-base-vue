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
        },
        {
          id: 2,
          name: 'Test2',
          price: 250,
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
