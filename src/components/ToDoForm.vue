<!--<form>侦听submit提交事件。
结果：调用onSubmit（）方法，检查新标签是否为空，然后发出todo添加事件，最后清除新标签<input>。-->

<!--提交表单时 ToDoForm 组件内的 onSubmit ()方法发出的 todo 添加事件。
结果: App.vue调用 addToDo ()方法将新的 todo 项添加到 ToDoItems 数组。-->
<template>
  <form @submit.prevent="onSubmit">
    <h2 class="label-wrapper">
      <label for="new-todo-item" class="label__lg">What needs to be done?</label>
    </h2>
    <input
      type="text"
      id="new-todo-item"
      name="new-todo"
      autocomplete="off"
      v-model.lazy.trim="label"
      class="input__lg"
    />
    <button type="submit" class="btn btn__primary btn__lg">Add</button>
  </form>
</template>

<script>
export default {
  methods: {
    onSubmit() {
      if (this.label == "") {
        return;
      }
      //自定义事件的发射方式
      this.$emit("todo-added", this.label);
      this.label = "";
    },
  },
  data() {
    return {
      label: "",
    };
  },
};
</script>

<style></style>
