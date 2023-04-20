<template>
  <table :class="tableClass">
    <thead>
      <tr>
        <template v-for="field in fields">
          <th
            :class="[{
              'cursor-pointer': isSortableField(field)},
              thClass,
            ]"
            :key="field.key"
            @click="onHeadClick(field)"
          >
            <div :class="thInnerClass">
              <slot name="thInner" :field="field">
                <span v-text="field.label" />
              </slot>
              <slot v-if="isActiveOrderBy(field.key) && !sortDesc" name="sortIconAsc" />
              <slot v-else-if="isActiveOrderBy(field.key) && sortDesc" name="sortIconDesc" />
              <slot v-else-if="isSortableField(field)" name="sortIconClassNotActive" />
            </div>
          </th>
        </template>
      </tr>
    </thead>
    <tbody>
      <tr v-if="isLoading">
        <td :colspan="fields.length">
          <slot name="loading" />
        </td>
      </tr>
      <tr v-else-if="items.length === 0">
        <td :colspan="fields.length">
          <slot name="empty">
            <div v-text="'Not found'"/>
          </slot>
        </td>
      </tr>
      <tr v-for="(item, key) in items" v-else :key="key">
        <template v-for="field in fields">
          <td
            :class="tdClass"
            :key="field.key"
          >
            <slot
              :index="key"
              name="td"
              :field="field.key"
              :item="field.key in item ? item[field.key] : null"
            >
              {{ item[field.key] }}
            </slot>
          </td>
        </template>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: 'TableBase',
  props: {
    /**
     * Field list
     */
    fields: {
      type: Array,
      required: true,
    },
    /**
     * Item list
     */
    items: {
      type: Array,
      required: true,
    },
    /**
     * Items are loading
     */
    isLoading: {
      type: Boolean,
      default: false,
    },
    /**
     * Order by field name
     */
    orderBy: {
      type: String,
      default: null,
    },
    /**
     * Table class
     */
    tableClass: {
      type: String,
      default: null,
    },
    /**
     * "th" element css lass
     */
    thClass: {
      type: String,
      default: null,
    },
    /**
     * "th" inner element css lass
     */
    thInnerClass: {
      type: String,
      default: null,
    },
    /**
     * "td" element css class
     */
    tdClass: {
      type: String,
      default: null,
    },
  },
  data() {
    return {
      /**
       * Sort is desc or not. Default not defined.
       */
      sortDesc: null,
    }
  },
  methods: {
    /**
     * Is order by active by field?
     *
     * @param field
     * @returns {boolean}
     */
    isActiveOrderBy(field) {
      return field === this.orderBy
    },
    /**
     * Is field sortable?
     *
     * @param field
     * @returns {boolean}
     */
    isSortableField(field) {
      return field.sort === undefined || field.sort
    },
    /**
     * Is sort defined?
     *
     * @returns {boolean}
     */
    isSortDescDefined() {
      return this.sortDesc !== null
    },
    /**
     * Set sort desc.
     *
     * @param field
     */
    setSortDesc(field) {
      if (this.isSortDescDefined() && !this.isOrderByChanged(field)) {
        this.sortDesc = !this.sortDesc
        return
      }

      this.sortDesc = false // default (first click) sort is ASC
    },
    /**
     * Is order by changed?
     *
     * @param field
     * @returns {boolean}
     */
    isOrderByChanged(field) {
      return this.orderBy !== field
    },
    /**
     * Table head clicked.
     */
    onHeadClick(field) {
      if (!this.isSortableField(field)) {
        return
      }
      this.setSortDesc(field.key)
      this.$emit('orderChanged', { sortDesc: this.sortDesc, orderBy: field.key })
    },
  },
}
</script>

<style scoped>
.cursor-pointer {
  cursor: pointer;
}
</style>
