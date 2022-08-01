<template>
  <div class="todos">
    <input
      type="text"
      v-model="newTodo"
      @keypress.enter="addTodo"
      placeholder="Add a new todo..."
    />
    <!-- Mode : We can say here 'in-out' or 'out-in'. What that does is say look I want you to either do the in transition first 
          and then the out one or the out one first and then m1. We're going to go without 'in'
            and that means that in that transition this will fade out first and then this will fade in and vice versa. -->
    <transition name="switch" mode="out-in">
      <div v-if="todos.length">
        <!--Appear : What that means is when the page first loads I want you to transition these in when they first appear.. -->
        <transition-group tag="ul" name="list" appear>
          <li v-for="todo in todos" :key="todo.id" @click="deleteTodo(todo.id)">
            {{ todo.text }}
          </li>
        </transition-group>
      </div>
      <div v-else>Woohoo, nothing left todo!</div>
    </transition>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup(props, { emit }) {
    const todos = ref([
      { text: "make the bed", id: 1 },
      { text: "play video games", id: 2 },
    ]);
    const newTodo = ref("");

    const addTodo = () => {
      if (newTodo.value) {
        const id = Math.random();
        todos.value = [{ text: newTodo.value, id }, ...todos.value];
        newTodo.value = "";
      } else {
        emit("badValue");
      }
    };

    const deleteTodo = (id) => {
      todos.value = todos.value.filter((todo) => todo.id != id);
    };

    return { todos, addTodo, deleteTodo, newTodo };
  },
};
</script>

<style>
.todos {
  max-width: 400px;
  margin: 20px auto;
  position: relative;
}
input {
  width: 100%;
  padding: 12px;
  border: 1px solid #eee;
  border-radius: 10px;
  box-sizing: border-box;
  margin-bottom: 20px;
}
.todos ul {
  position: relative;
  padding: 0;
}
.todos li {
  list-style-type: none;
  display: block;
  margin-bottom: 10px;
  padding: 10px;
  background: white;
  box-shadow: 1px 3px 5px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  width: 100%;
  box-sizing: border-box;
}
.todos li:hover {
  cursor: pointer;
}

/* list transitions */
.list-enter-from {
  opacity: 0;
  transform: scale(0.6);
}
.list-enter-active {
  transition: all 0.4s ease;
}
.list-leave-to {
  opacity: 0;
  transform: scale(0.6);
}

/*
  if we add items it's works but if we remove items it's doesn't work, it still jumps up.
  The reason is because we have to do one more thing and that is to come to 'list-leave-active' class and set a position to 'absolute'. 

  When we absolutely position that it takes out of the normal document flow so the item we're removing is taken out of normal document flow
  and therefore we can use a transition under that to move the rest up
  */
.list-leave-active {
  transition: all 0.4s ease;
  position: absolute; /* for move transition after item leaves */
}

/* move : What this does is use a 'transform' under the hood to move it into its new position */
/* How to animate these items after something is deleted or when something added ? */
.list-move {
  transition: all 0.3s ease;
}

/* switch transitions */
.switch-enter-from,
.switch-leave-to {
  opacity: 0;
  transform: translateY(20px);
}
.switch-enter-active {
  transition: all 0.5s ease;
}

.switch-leave-active {
  transition: all 0.5s ease;
  position: absolute;
  width: 100%;
}
</style>
