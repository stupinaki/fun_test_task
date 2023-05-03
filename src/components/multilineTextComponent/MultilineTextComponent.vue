<template>
  <div>
    <div v-for="(str, i) in array" :key="str + i">
      {{ str }}
    </div>
  </div>
</template>

<script>
export default {
  name: "MultilineTextComponent",
  props: {
    value: {
      type: String,
      required: true,
    }
  },
  computed: {
    array() {
      return this.$props.value
          .split('[')
          .filter(line => line)
          .map(line => "[" + line.slice(0));
    }
  }
}
</script>
