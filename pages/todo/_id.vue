<template>
  <v-card width="600" max-width="90%">
    <v-btn block class="mb-3" @click="$router.push('/')">Go Back</v-btn>

    <v-card-title>Todo {{ todo.name }}</v-card-title>

    <v-text-field
      @keyup.enter="addTodoItem({ todoIndex: id, name: todoItemName })"
      v-model="todoItemName"
      placeholder="Create Todo Item"
    />
    <v-list two-line>
      <v-slide-x-transition group>
        <v-list-item v-for="(item, index) in todo.array" v-bind:key="index">
          <v-list-item-title>
            <v-btn
              v-bind:class="{ 'text-decoration-line-through': item.done }"
              block
              @click="
                checkTodoItem({
                  todoIndex: id,
                  itemIndex: index
                })
              "
              >{{ index + 1 }}. {{ item.name }}</v-btn
            >
          </v-list-item-title>

          <v-btn
            dark
            @click="
              removeTodoItem({
                todoIndex: id,
                itemIndex: index
              })
            "
          >
            Delete
          </v-btn>
        </v-list-item>
      </v-slide-x-transition>
    </v-list>
  </v-card>
</template>

<script>
export default {
  computed: {
    todo() {
      return this.$store.state.todos.array[this.id];
    },
    id() {
      return this.$route.params.id;
    }
  },
  data() {
    return {
      todoItemName: ""
    };
  },
  methods: {
    addTodoItem(payload) {
      if (this.todoItemName.length > 0) {
        this.$store.commit("todos/addTodoItem", payload);
        this.todoItemName = "";
      }
    },
    checkTodoItem(payload) {
      this.$store.commit("todos/checkTodoItem", payload);
    },
    removeTodoItem(payload) {
      this.$store.commit("todos/removeTodoItem", payload);
    }
  }
};
</script>
