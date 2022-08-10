<template>
  <form class="stack-small" @submit.prevent="onSubmit">
    <div>
      <label class="edit-label">Edit Name for &quot;{{ label }}&quot;</label>
      <input :id="id" type="text" autocomplete="off" v-model.lazy.trim="newLabel" ref="labelInput" />
    </div>

    <div class="btn-group">
      <button type="button" class="btn" @click="onCancel">
        Cancel<span class="visually-hidden">editing {{ label }}</span>
      </button>

      <!--当 ToDoItemEditForm 内的 onSubmit ()方法发出的item-edited被成功监听时，
      ToDoItem 组件内的 itemEdited()方法也发出item-edited事件。-->
      <button type="submit" class="btn btn__primary">
        Save<span class="visually-hidden">edit for {{ label }}</span>
      </button>
    </div>
  </form>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      required: true,
    },
    id: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      newLabel: this.label,
    };
  },
  methods: {
    onSubmit() {
      //* !==非全等
      if (this.newLabel && this.newLabel !== this.label) {
        this.$emit("item-edited", this.newLabel);
      }
    },
    onCancel() {
      this.$emit("edit-cancelled");
    },
  },
  mounted() {
    const labelInputRef = this.$refs.labelInput;
    labelInputRef.focus();
  },
};
</script>

<style scoped>
.edit-label {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #0b0c0c;
  display: block;
  margin-bottom: 5px;
}
input {
  display: inline-block;
  margin-top: 0.4rem;
  width: 100%;
  min-height: 4.4rem;
  padding: 0.4rem 0.8rem;
  border: 2px solid #565656;
}
form {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}
form > * {
  flex: 0 0 100%;
}
</style>
