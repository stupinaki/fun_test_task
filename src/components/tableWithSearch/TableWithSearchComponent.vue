<template>
  <DataTable :value="searchResult">
    <template #header>
      <div class="table-with-search-headers">
        <SelectComponent
            :value="headers"
            :is-chip="false"
            optionLabel="header"
            placeholder="Искать по.."
            :selected-props-value="selectedColumnsForSearch"
            @select-update="onSearchColumnsUpdate"
        />
        <form class="p-inputgroup" @submit.prevent="initSearch">
          <div class="p-input-icon-right">
            <InputText
                v-model="searchText"
                placeholder="Поиск"
            />
            <i
                v-show="searchText"
                class="pi pi-times-circle"
                @click="clearSearchText"
            />
          </div>
          <Button label="Search" @click="initSearch"/>
        </form>
        <SelectComponent
            :value="headers"
            optionLabel="header"
            :is-chip="false"
            placeholder="Поля Таблицы"
            :selected-props-value="selectedHeadersValue"
            @select-update="onSelectUpdate"
        />
      </div>
    </template>
    <Column
        v-for="header in selectedHeadersValue"
        :key="header.field"
        :field="header.field"
        :header="header.header"
    />
  </DataTable>
</template>

<script>
import MultilineTextComponent from "@/components/multilineTextComponent/MultilineTextComponent.vue";
import SelectComponent from "../selectComponent/SelectComponent.vue";
import DataTable from "primevue/datatable";
import InputText from "primevue/inputtext";
import Column from "primevue/column";
import Button from "primevue/button";

export default {
  name: "TableWithSearchComponent",
  components: {
    MultilineTextComponent,
    SelectComponent,
    DataTable,
    Column,
    InputText,
    Button
  },
  data() {
    return {
      products: [
        {
          name: "name 1",
          description: "some description",
          count: 2234,
          isMultiline: false,
        },
        {
          name: "some name 2",
          description: "some description 2",
          count: 21,
          isMultiline: true,
        }
      ],
      headers: [
        {field: "name", header: "Name"},
        {field: "description", header: "Description"},
        {field: "count", header: "Count"}
      ],
      selectedHeadersValue: [
        {field: "name", header: "Name"},
        {field: "description", header: "Description"},
        {field: "count", header: "Count"}
      ],
      searchText: undefined,
      searchResult: [
        {
          name: "name 1",
          description: "some description",
          count: 2234,
          isMultiline: false,
        },
        {
          name: "some name 2",
          description: "some description 2",
          count: 21,
          isMultiline: true,
        }
      ],
      selectedColumnsForSearch: []
    }
  },
  methods: {
    onSelectUpdate(value) {
      this.$data.selectedHeadersValue = value;
    },
    onSearchColumnsUpdate(value) {
      this.$data.selectedColumnsForSearch = value;
    },
    initSearch() {
      const { searchText, products, selectedHeadersValue, selectedColumnsForSearch } = this.$data;
       if(selectedColumnsForSearch.length) {
         const selectedHeaders = selectedColumnsForSearch.map(h => h.field);
         this.$data.searchResult = products.filter(p => {
           return selectedHeaders.some(h => {
             const value = p[h].toString().toLowerCase();
             return value.includes(searchText.toLowerCase());
           })
         })
       } else {
         const headers = selectedHeadersValue.map(h => h.field);
         this.$data.searchResult = products.filter(p => {
           return headers.some(h => {
             const value = p[h].toString().toLowerCase();
             return value.includes(searchText.toLowerCase());
           })
         })
       }
    },
    clearSearchText() {
      this.$data.searchText = "";
    },
  },
}
</script>

<style scoped>
.table-with-search-headers {
  display: grid;
  grid-template-columns: calc(20% - 4px) calc(60% - 4px) calc(20% - 4px);
  gap: 12px;
}
.p-inputtext {
  width: 100%;
  height: 100%;
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
}
.p-input-icon-right {
  width: 100%;
}
</style>