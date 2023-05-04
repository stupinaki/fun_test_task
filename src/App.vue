<template>
  <div class="app">
    <LinkComponent />
    <DataTimePickerComponent
        :selected-time="selectedTime"
        :show-icon="true"
        @time-update="onTimeUpdate"
    />
    <SelectComponent
        :value="selectValue"
        :selected-props-value="selectedSelectValue"
        option-label="name"
        @select-update="onSelectUpdate"
    />
    <MultilineTextTable/>
    <TableWithSearchComponent/>
  </div>
</template>

<script>
import 'primeicons/primeicons.css';
import 'primevue/resources/primevue.min.css';
import 'primevue/resources/themes/saga-blue/theme.css';
import LinkComponent from "@/components/linkComponent/LinkComponent.vue";
import SelectComponent from "@/components/selectComponent/SelectComponent.vue";
import MultilineTextTable from "@/components/multilineTextComponent/MultilineTextTable.vue";
import DataTimePickerComponent from "@/components/dateTimePickerComponent/DateTimePickerComponent.vue";
import TableWithSearchComponent from "./components/tableWithSearch/TableWithSearchComponent.vue";

export default {
  name: "App",
  components: {
    LinkComponent,
    SelectComponent,
    MultilineTextTable,
    DataTimePickerComponent,
    TableWithSearchComponent
  },
  data() {
    return {
      selectValue: [
        {name: 'New York', code: 'NY'},
        {name: 'Rome', code: 'RM'},
        {name: 'London', code: 'LDN'},
        {name: 'Istanbul', code: 'IST'},
        {name: 'Paris', code: 'PRS'}
      ],
      selectedTime: new Date(),
      selectedSelectValue: [],
    }
  },
  methods: {
    onTimeUpdate(value) {
      console.log("time update, new time:", value);
    },
    onSelectUpdate(value) {
      this.$data.selectedSelectValue = value;
      console.log("select update, new value:", value);
    }
  }
}

</script>

<style>
.app {
  display: flex;
  flex-direction: column;
  gap: 40px;
  margin: 40px 0;
}
</style>