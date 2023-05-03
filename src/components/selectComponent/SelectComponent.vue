<template>
  <div class="card flex justify-content-center">
    <MultiSelect
        v-model="selectedValue"
        :options="value"
        :display="display"
        :placeholder="placeholder"
        :optionLabel="optionLabel"
        :maxSelectedLabels="maxSelectedLabels"
        @update:modelValue="onValueUpdate"
    >
      <template #closeicon>
        <i
            v-show="selectedValue.length"
            @click="clearAll"
            class="pi pi-times-circle"
        />
      </template>
    </MultiSelect>
  </div>
</template>

<script>
import MultiSelect from "primevue/multiselect";

export default {
  name: "SelectComponent",
  emits: ["selectUpdate"],
  components: {
    MultiSelect,
  },
  data() {
    return {
      selectedValue: [],
    }
  },
  props: {
    value: {
      type: Array,
      required: true,
    },
    selectedPropsValue: {
      type: Array,
      required: false,
    },
    optionLabel: {
      type: String,
      required: true,
    },
    placeholder: {
      type: String,
      required: false,
      default: "Select values"
    },
    isChip: {
      type: Boolean,
      required: false,
      default: true
    },
    maxSelectedLabels: {
      type: Number,
      required: false,
      default: 3
    }
  },
  methods: {
    clearAll() {
      this.$emit("selectUpdate", []);
    },
    onValueUpdate() {
      this.$emit("selectUpdate", this.$data.selectedValue);
    }
  },
  watch: {
    selectedPropsValue() {
      this.$data.selectedValue = this.$props.selectedPropsValue;
    }
  },
  beforeMount() {
    this.$data.selectedValue = this.$props.selectedPropsValue;
  },
  computed: {
    display() {
      return this.$props.isChip ? "chip" : "comma";
    }
  }
}
</script>

<style>
.p-multiselect {
  width: 100%;
}
</style>
