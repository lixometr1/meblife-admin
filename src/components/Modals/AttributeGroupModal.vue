<template>
  <Modal title="Группа атрибутов" @close="$emit('close')">
    <template>
      <CCard>
        <CCardHeader>Редактировать группу атрибутов</CCardHeader>
        <CCardBody>
          <TInput label="Название" v-model="item.name"></TInput>
        </CCardBody>
      </CCard>
    </template>
    <template #footer>
      <div class="d-flex justify-content-end">
        <CButton class="mr-2" color="secondary" @click="$emit('close')">Отменить</CButton>

        <CButton  color="success" @click="save">Сохранить</CButton>
      </div>
    </template>
  </Modal>
</template>

<script>
import Modal from "@/components/Modals/Modal";
import TInput from "@/components/TInput";
export default {
  props: {
    new: Boolean,
    id: String,
  },
  components: {
    Modal,
    TInput,
  },
  data() {
    return {
      isNew: this.new,
      thisId: this.id,
      item: {
        name: [],
      },
    };
  },
  async created() {
    await this.fetch();
  },
  methods: {
    async fetch() {
      this.isLoading = true;
      try {
        if (!this.isNew) {
          const { data } = await this.$api.get("attributeGroupByIdAdmin", {
            id: this.thisId,
          });
          this.item = data;
        }
      } catch (err) {
        this.$error(err);
      }
      this.isLoading = false;
    },
    async save() {
      if (this.isSaving) return;
      this.isSaving = true;
      try {
        if (this.isNew) {
          const { data } = await this.$api.post(
            "attributeGroups",
            null,
            this.item
          );
          this.item = data;
          this.thisId = this.item._id;
          this.$notify({
            group: "main",
            type: "success",
            text: "Сохранено!",
          });
          this.isNew = false;
        } else {
          const { data } = await this.$api.put(
            "attributeGroupById",
            { id: this.thisId },
            this.item
          );
          this.$notify({
            group: "main",

            type: "success",
            text: "Сохранено!",
          });
        }
      } catch (err) {
        this.$error(err);
      }
      this.isSaving = false;
    },
  },
  watch: {
    id() {
      this.thisId = this.id;
    },
  },
};
</script>

<style lang="scss" scoped>
</style>