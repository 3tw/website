<template>
  <b-table
    responsive
    bordered
    outlined
    hover
    :stickyColumn="' '"
    :sort-desc="true"
    :sticky-header="tableHeight"
    :items="items"
    :fields="fields"
  >
    <template v-slot:head()="scope">
      <div class="text-nowrap">{{ scope.label }}</div>
    </template>
    <template v-slot:cell(date)="data">
      <div class="text-nowrap">{{ data.item.date | formatDate('dd. MMMM') }}</div>
    </template>
    <template v-slot:empty="scope">
      <h4>{{ scope.emptyText }} ni podatka</h4>
    </template>
  </b-table>
</template>

<script>
import { mapGetters } from "vuex";
import _ from "lodash";
export default {
  props: ["tableHeight"],
  data() {
    return {
      items: [],
      fields: [],
      dimensions: [
        "tests.performed",
        "tests.performed.todate",
        "cases.confirmed",
        "cases.confirmed.todate",
        "cases.hs.employee.confirmed.todate",
        "cases.rh.employee.confirmed.todate",
        "cases.rh.occupant.confirmed.todate",
        "cases.unclassified.confirmed.todate",
        "cases.active",
        "cases.recovered.todate",
        "cases.closed.todate",
        "state.in_hospital",
        "state.icu",
        "state.critical",
        "state.in_hospital.todate",
        "state.out_of_hospital.todate",
        "state.deceased.todate",
      ]
    };
  },
  watch: {
    tableData() {
      this.refreshData()
    }
  },
  computed: {
    ...mapGetters("tableData", ["tableData", "filterTableData"])
  },
  methods: {
    refreshData(){
      const { items, fields } = this.filterTableData(this.dimensions);
      this.items = items;
      this.fields = fields;
    }
  },
  mounted(){
    this.refreshData()
  }
};
</script>
