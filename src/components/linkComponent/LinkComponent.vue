<template>
  <div>
    <form @submit.prevent="fetchTitle">
      <InputText
          v-if="isEdit"
          type="text"
          v-model.trim="inputValue"
          placeholder="https://"
          @blur="fetchTitle"
      />

      <div v-else class="link-edit-error-container">
        <span v-if="isError" class="link-edit-error">
          Unable to get a site title for the link:
        </span>
        <div class="link-edit-container">
          <a :href="inputValue" target="_blank">
            {{ siteTitle }}
          </a>
          <i class="pi pi-file-edit" @click="onClick"/>
        </div>
      </div>

    </form>
  </div>
</template>

<script>
import InputText from "primevue/inputtext";

export default {
  name: "LinkComponent",
  components: {
    InputText,
  },
  data() {
    return {
      inputValue: undefined,
      linkTitle: undefined,
      isLoading: false,
      isError: false,
      isEdit: true,
    }
  },
  computed: {
    siteTitle() {
      const {inputValue, linkTitle, isLoading, isError} = this.$data;
      if (isError) {
        return inputValue;
      }
      if (isLoading) {
        return "Loading a site title...";
      }
      return linkTitle;
    }
  },
  methods: {
    async fetchTitle() {
      const url = this.$data.inputValue;
      if (!url) {
        return;
      }
      this.$data.isLoading = true;
      this.$data.isEdit = false;
      this.$data.isError = false;
      try {
        const response = await fetch(`https://api.allorigins.win/get?url=${encodeURIComponent(url)}`)
        if (response.status !== 200) {
          this.$data.isError = true;
          this.$data.isLoading = false;
          return;
        }
        const content = await response.text();
        const {groups} = /<title(.*)>(?<title>.*?)<\/title>/.exec(content);
        this.$data.isLoading = false;
        this.$data.linkTitle = groups.title;
      } catch (error) {
        this.$data.isError = true;
        console.log("error:", error);
      }
    },
    onClick() {
      this.$data.isEdit = true;
    },
  },

}
</script>

<style scoped>
.link-edit-error-container {
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.link-edit-error {
  font-size: 12px;
  color: red;
}
.link-edit-container {
  display: flex;
  align-items: center;
  gap: 20px;
  cursor: pointer;
}

.p-inputtext {
  width: 100%;
}
</style>