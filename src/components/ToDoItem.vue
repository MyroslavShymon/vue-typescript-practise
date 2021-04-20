<template>
  <li>
    <div>
      <input v-model="checked" type="checkbox" />
    </div>
    <div>
      <span
        :class="{ completed: todo.completed }"
        v-if="!editable"
        @click="editable = !editable"
      >
        {{ todo.text ? todo.text : "Click to edit Todo" }}
      </span>
      <input
        v-else
        type="text"
        :value="todo.text"
        @input="onTextChange"
        v-on:keyup.enter="editable = !editable"
      />
    </div>

    <div>
      <button @click="editable = !editable">
        {{ editable ? "Save" : "Edit" }}
      </button>
      <button @click="$emit('remove-todo', todo)">Delete</button>
    </div>
  </li>
</template>

<script lang="ts">
//   export default defineComponent({
// name: "TodoItem",
//     props: {
//       todo: {
//         type: Object as PropType<ToDo>,
//         required: true
//       }
//     },
//     data() {
//       return {
//         editable: false
//       }
//     },
//     methods: {
//       onTextChange(e: { target: { value: string } }) {
//         this.$emit("update-todo", e.target.value)
//       }
//     },
//     computed: {
//       checked: {
//         get(): boolean {
//           return this.todo.completed
//         },
//         set(value: boolean) {
//           this.$emit("checkbox-click", value)
//         }
//       }
//     }
//   })
import { PropType } from "vue";
import ToDo from "@/models/ToDoModel";
import { Component, Prop, Vue } from "vue-property-decorator";

@Component({
  name: "TodoItem",
  data() {
    return {
      //   firstNumber: 0,
      editable: false,
    };
  },
})
export default class ToDoItem extends Vue {
  private firstNumber!: number;
  @Prop({ type: Object as PropType<ToDo>, required: true })
  private todo!: Record<string, unknown>;

  onTextChange(e: { target: { value: string } }) {
    this.$emit("update-todo", e.target.value);
  }

  get checked(): boolean {
    return !this.todo.completed;
  }

  set checked(value: boolean) {
    this.$emit("checkbox-click", value);
  }
}
</script>

<style scoped>
.completed {
  text-decoration: line-through;
}
</style>
