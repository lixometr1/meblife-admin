<template>
  <div>
    <SearchSelect
      :label="label"
      :value="value"
      @input="$emit('input', $event)"
      :multiple="multiple"
      :findItem="findItem"
      :searchItem="searchItem"
    >
      <template #list-footer>
        <CButton
          class="w-100 mt-2"
          variant="outline"
          color="primary"
          @click="addGroup"
        >Добавить группу</CButton>
      </template>
    </SearchSelect>
  </div>
</template>

<script>
import SearchSelect from "@/components/SearchSelect";
import AttributeGroupModal from "@/components/Modals/AttributeGroupModal";
export default {
  props: {
    value: [String, Array],
    label: String,
    multiple: {
      type: Boolean,
      default: false,
    },
  },
  components: {
    SearchSelect,
  },
  methods: {
    addGroup() {
      this.$modal.show(
        AttributeGroupModal,
        {new: true},
        { width: "100%", height: "100%" }
      );
    },
    async findItem(id) {
      const { data: item } = await this.$api.get("attributeGroupById", { id });
      return item;
    },
    async searchItem(text, options) {
      const { data } = await this.$api.get("attributeGroupsSearch", {text}, {params: options});
      return data
    },
  },
};
</script>

<style lang="scss">
</style>