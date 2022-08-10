<template>
  <!--创建组件模板部分,模板目前只允许一个根元素-->
  <div class="stack-small" v-if="!isEditing">
    <div class="custom-checkbox">
      <!--当勾选 ToDoItem 组件内部的复选框时，发出 checkbox-change 事件。
      结果: 调用 updateDoneStatus ()方法来更新相关 todo 项的完成状态（done状态）。-->
      <input type="checkbox" :id="id" :checked="isDone" class="checkbox" @change="$emit('checkbox-changed')" />
      <label :for="id" class="checkbox-label">{{ label }}</label>
    </div>

    <div class="btn-group">
      <!--当 ToDoItemEditForm 内的 onSubmit ()方法发出的项目编辑事件被成功监听时，ToDoItem 组件内的 itemEditing ()方法发出的项目编辑事件。是的，这是两个不同的项目编辑事件链！
      结果: 调用 edit ToDo ()方法来更新相关 todo 项的标签。-->
      <button type="button" class="btn" @click="toggleToItemEditForm" ref="editButton">
        Edit<span class="visually-hidden">{{ label }}</span>
      </button>

      <!--按下“删除”按钮时，ToDoItem组件内的deleteToDo（）方法发出的item-deleted事件。
      结果：调用deleteToDo（）方法删除关联的todo项。-->
      <button type="button" class="btn btn__danger" @click="deleteToDo">
        Delete<span class="visually-hidden">{{ label }}</span>
      </button>
    </div>
  </div>

  <!--当 ToDoItemEditForm 内的 onSubmit ()方法发出的item-edited被成功监听时，
      ToDoItem 组件内的 itemEdited()方法也发出item-edited事件。-->
  <to-do-item-edit-form
    v-else
    :id="id"
    :label="label"
    @item-edited="itemEdited"
    @edit-cancelled="editCancelled"
  ></to-do-item-edit-form>
</template>

<script>
import ToDoItemEditForm from "./ToDoItemEditForm.vue";
//import uniqueId from 'lodash.uniqueid';

//默认导出对象，即组件对象
export default {
  components: { ToDoItemEditForm },
  props: {
    //required告诉 Vue 说，我们希望每个该组件的实例都必须有个 label 字段。如果 ToDoItem 组件没有 label 字段的话，Vue 会提示警告。
    label: { required: true, type: String },
    //当没有 done prop 被传递给 ToDoItem 组件时， done prop 的值会是 false
    done: { default: false, type: Boolean },
    id: { required: true, type: String },
  },
  data() {
    return {
      //isDone: this.done,
      isEditing: false,
      //id: uniqueId('todo-')
    };
  },
  computed: {
    isDone() {
      return this.done;
    },
  },
  methods: {
    deleteToDo() {
      this.$emit("item-deleted");
    },
    toggleToItemEditForm() {
      //console.log(this.$refs.editButton);
      this.isEditing = true;
    },
    itemEdited(newLabel) {
      this.$emit("item-edited", newLabel);
      this.isEditing = false;
      this.focusOnEditButton();
    },
    editCancelled() {
      this.isEditing = false;
      this.focusOnEditButton();
    },
    focusOnEditButton() {
      //因为当按了save或者cancel之后，edit按钮还没有被生成出来，所以应该在下一个生命周期里focus
      this.$nextTick(() => {
        const editButtonRef = this.$refs.editButton;
        editButtonRef.focus();
      });
    },
  },
};
</script>

<style scoped>
.custom-checkbox > .checkbox-label {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: 400;
  font-size: 16px;
  font-size: 1rem;
  line-height: 1.25;
  color: #0b0c0c;
  display: block;
  margin-bottom: 5px;
}
.custom-checkbox > .checkbox {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-weight: 400;
  font-size: 16px;
  font-size: 1rem;
  line-height: 1.25;
  box-sizing: border-box;
  width: 100%;
  height: 40px;
  height: 2.5rem;
  margin-top: 0;
  padding: 5px;
  border: 2px solid #0b0c0c;
  border-radius: 0;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
}
.custom-checkbox > input:focus {
  outline: 3px dashed #fd0;
  outline-offset: 0;
  box-shadow: inset 0 0 0 2px;
}
.custom-checkbox {
  font-family: Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  font-weight: 400;
  font-size: 1.6rem;
  line-height: 1.25;
  display: block;
  position: relative;
  min-height: 40px;
  margin-bottom: 10px;
  padding-left: 40px;
  clear: left;
}
.custom-checkbox > input[type="checkbox"] {
  -webkit-font-smoothing: antialiased;
  cursor: pointer;
  position: absolute;
  z-index: 1;
  top: -2px;
  left: -2px;
  width: 44px;
  height: 44px;
  margin: 0;
  opacity: 0;
}
.custom-checkbox > .checkbox-label {
  font-size: inherit;
  font-family: inherit;
  line-height: inherit;
  display: inline-block;
  margin-bottom: 0;
  padding: 8px 15px 5px;
  cursor: pointer;
  touch-action: manipulation;
}
.custom-checkbox > label::before {
  content: "";
  box-sizing: border-box;
  position: absolute;
  top: 0;
  left: 0;
  width: 40px;
  height: 40px;
  border: 2px solid currentColor;
  background: transparent;
}
.custom-checkbox > input[type="checkbox"]:focus + label::before {
  border-width: 4px;
  outline: 3px dashed #228bec;
}
.custom-checkbox > label::after {
  box-sizing: content-box;
  content: "";
  position: absolute;
  top: 11px;
  left: 9px;
  width: 18px;
  height: 7px;
  transform: rotate(-45deg);
  border: solid;
  border-width: 0 0 5px 5px;
  border-top-color: transparent;
  opacity: 0;
  background: transparent;
}
.custom-checkbox > input[type="checkbox"]:checked + label::after {
  opacity: 1;
}
@media only screen and (min-width: 40rem) {
  label,
  input,
  .custom-checkbox {
    font-size: 19px;
    font-size: 1.9rem;
    line-height: 1.31579;
  }
}
</style>
