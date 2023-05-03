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
      const { value } = this.$props;
      const newStr = value.replace(/\[/g, "   [");
      return newStr.split('   ').map(s => s.trim() && s).filter(s => s);
    }
  }
}
</script>
