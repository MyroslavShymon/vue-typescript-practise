<template>
  <div class="note-page">
    <input :value="note.title" @input="updateTitle" />
    <h2>{{ note.title }}</h2>
    <hr />
    <ul>
      <TodoItem
        v-for="(todo, index) in note.todos"
        :todo="todo"
        :key="index"
        @remove-todo="onRemoveTodo(index)"
        @update-todo="onUpdateTodo($event, index)"
        @checkbox-click="onCheckboxClick($event, index)"
      />
    </ul>
    <div class="new-todo">
      <button @click="addNewTodo">Add Todo</button>
      <span @click="addNewTodo">Add New Todo</span>
    </div>
    <hr />
    <div>
      <button @click="saveNote">Save</button>
      <button @click="cancelEdit">Cancel</button>
      <button @click="DeleteNote">Delete</button>
    </div>
    <hr />
  </div>
</template>

<script lang="ts">
import TodoItem from "@/components/ToDoItem.vue";
import Note from "@/models/NoteModel";
import ToDo from "@/models/ToDoModel";
import store from "@/store";
import router from "@/router";
import { Component, Prop, Vue } from "vue-property-decorator";

@Component({
  name: "Note",
  components: {
    TodoItem,
  },
  mounted() {
    const { currentRoute } = router;
    if (currentRoute.params.id) {
      const routeId: number = +currentRoute.params.id;
      store.dispatch("fetchCurrentNote", routeId);
    } else {
      const id = store.getters.getIdOfLastNote + 1;
      store.commit("setCurrentNote", {
        title: "",
        todos: [] as ToDo[],
        id: id,
      });
    }
  },

  data() {
    return {};
  },
})
export default class NoteComponent extends Vue {
  get note() {
    return store.state.currentNote;
  }
  saveNote() {
    store.dispatch("saveNote");
    router.push("/");
  }
  DeleteNote() {
    store.commit("deleteNote", this.note);
    router.push("/");
  }

  updateTitle = (e: { target: { value: string } }) => {
    store.commit("updateTitle", e.target.value);
  };

  addNewTodo() {
    store.commit("addNewTodo");
  }

  onRemoveTodo = (index: number) => {
    store.commit("deleteTodo", index);
  };
  onUpdateTodo = (text: string, index: number) => {
    let todos = [...store.state.currentNote.todos];

    todos[index] = { ...todos[index], text };
    store.commit("updateTodos", todos);
  };
  onCheckboxClick = (completed: boolean, index: number) => {
    let todos = [...store.state.currentNote.todos];

    todos[index] = { ...todos[index], completed };
    store.commit("updateTodos", todos);
  };

  cancelEdit() {
    // undo changes
    router.push("/");
  }
  clearNote() {
    const id = store.getters.getIdOfLastNote + 1;
    store.commit("setCurrentNote", {
      title: "",
      todos: [] as ToDo[],
      id: id,
    } as Note);
  }

  //   beforeRouteLeave(to, from, next) {
  //     this.clearNote();
  //     next();
  //   }
}
//   export default defineComponent({
// name: "Note",
//     components: {
//       TodoItem
//     },
//     setup() {
//       const note = computed(() => store.state.currentNote)

//       const saveNote = () => {
//         store.dispatch("saveNote")
//         router.push("/")
//       }
//       const DeleteNote = () => {
//         store.commit("deleteNote", note)
//         router.push("/")
//       }

//       const { currentRoute } = router
//       const fetchNote = () => {
//         if (currentRoute.value.params.id) {
//           const routeId: number = +currentRoute.value.params.id
//           store.dispatch("fetchCurrentNote", routeId)
//         } else {
//           const id = store.getters.getIdOfLastNote + 1
//           store.commit("setCurrentNote", {
//             title: "",
//             todos: [] as ToDo[],
//             id: id
//           })
//         }
//       }
//       onMounted(fetchNote)

//       const updateTitle = (e: { target: { value: string } }) => {
//         store.commit("updateTitle", e.target.value)
//       }

//       const addNewTodo = () => {
//         store.commit("addNewTodo")
//       }

//       const onRemoveTodo = (index: number) => {
//         store.commit("deleteTodo", index)
//       }
//       const onUpdateTodo = (text: string, index: number) => {
//         let todos = [ ...store.state.currentNote.todos ]

//         todos[index] = { ...todos[index], text }
//         store.commit("updateTodos", todos)
//       }
//       const onCheckboxClick = (completed: boolean, index: number) => {
//         let todos = [ ...store.state.currentNote.todos ]

//         todos[index] = { ...todos[index], completed }
//         store.commit("updateTodos", todos)
//       }

//       const cancelEdit = () => {
//         // undo changes
//         router.push("/")
//       }
//       const clearNote = () => {
//         const id = store.getters.getIdOfLastNote + 1
//         store.commit("setCurrentNote", {
//           title: "",
//           todos: [] as ToDo[],
//           id: id
//         } as Note)
//       }

//       return {
//         note,
//         saveNote,
//         addNewTodo,
//         cancelEdit,
//         onRemoveTodo,
//         onUpdateTodo,
//         DeleteNote,
//         clearNote,
//         updateTitle,
//         onCheckboxClick
//       }
//     },
//     beforeRouteLeave(to, from, next) {
//       this.clearNote()
//       next()
//     }
//   })
</script>

<style>
.new-todo {
  display: flex;
  justify-content: flex-start;
  background-color: #e2e2e2;
  height: 36px;
  margin: 5px 0px;
  padding-top: 4px;
  padding-left: 10px;
  padding-right: 15px;
  border-radius: 5px;
}
</style>
