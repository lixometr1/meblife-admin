<template>
  <Label :label="label">
    <LangEditor
      class="mb-4"
      v-for="(lang, idx) in languages"
      :key="idx"
      :value="langValue(lang._id)"
      @input="changeLanguage($event)"
    />
  </Label>
</template>

<script>
import { mapGetters } from "vuex";
import LangEditor from "./LangEditor";
export default {
  props: {
    label: String,
    /*
      [{value: '', langId: ''}]
    */
    value: Array,
  },
  components: {
    LangEditor,
  },

  computed: {
    horizontal() {
      return this.$store.getters.horizontal;
    },
    languages() {
      return this.$store.getters["language/activeLanguages"];
    },
    langValue() {
      return (langId) => {
        return (
          this.value.find((val) => val.langId === langId) || {
            langId,
            value: "",
          }
        );
      };
    },
  },
  methods: {
    changeLanguage(val) {
      const newValue = [...this.value];
      const idx = this.value.findIndex((v) => v.langId === val.langId);
      if (idx < 0) {
        newValue.push(val);
      } else {
        newValue[idx] = val;
      }
      this.$emit("input", newValue);
    },
  },
};
</script>

<style lang="scss" >
</style>