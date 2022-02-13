<template>
  <div class="todo" :class="{ dark_mode: darkMode }">
    <!-- [------------------------] -->
    <input class="toggle" type="checkbox" @click="darkMode = !darkMode" />

    <div class="container">
      <div class="box">
        <h1 class="title">~ Today I need to ~</h1>

        <div class="add_todo">
          <input
            type="text"
            placeholder="Add new to do..."
            class="todo_input"
            v-model="todo"
            @keyup.enter="addTodo()"
          />
          <button type="submit" class="submit_btn" @click="addTodo()">
            <span>Submit</span>
          </button>
        </div>

        <button
          type="submit"
          class="submit_btn remove_btn"
          @click="removeAllTodo()"
        >
          <span>Remove Todos</span>
        </button>

        <div class="todoList">
          <p v-if="todoBlank" class="todoError">{{ todoBlankError }}</p>
          <ul v-for="(item, index) in todoList" :key="item">
            <div class="libox">
              <input
                type="checkbox"
                id="todo"
                name="todo"
                class="todo_checkbox"
              />
              <label for="todo" :data-content="item">{{ item }}</label>
            </div>
            <i class="bx bx-x" @click="removeTodo(index)"></i>
          </ul>
        </div>

        <div class="footer" v-if="todoList.length == 0">
          <p class="todoError">Congrats, you have no more tasks to do!</p>
        </div>
      </div>
    </div>
    <!-- [------------------------] -->
    <footer>
      Developed by
      <a href="https://github.com/yakupsevik" target="_blank">Yakup Sevik</a>
    </footer>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      darkMode: false,
      todo: "",
      todoList: [],
      todoBlank: false,
      todoBlankError: "You didn't write a to do!",
    };
  },
  methods: {
    addTodo() {
      if (this.todo != "") {
        this.todoList.push(this.todo);
        localStorage.setItem("todo", JSON.stringify(this.todoList));
        this.todoBlank = false;
        this.todo = "";
      } else {
        this.todoBlank = true;
      }
    },
    removeTodo(index) {
      this.todoList.splice(index, 1);
      localStorage.setItem("todo", JSON.stringify(this.todoList));
    },
  },
  created() {
    let localStorageTodoList = localStorage.getItem("todo");
    if (localStorageTodoList != null)
      this.todoList = JSON.parse(localStorageTodoList);
  },
};
</script>

<style lang="scss">
@import url("../assets/css/Home.scss");

/* width */
::-webkit-scrollbar {
  width: 10px;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: rgb(255, 0, 106);
  border-radius: 10px;
}

.libox {
  display: flex;
  justify-self: center;
  align-self: center;
}

.todo_checkbox {
  position: relative;
  width: 1.5em;
  height: 1.5em;
  color: #fff;
  border: 1px solid #252525;
  border-radius: 4px;
  appearance: none;
  outline: 0;
  cursor: pointer;
  margin-right: 15px;
  transition: background 175ms cubic-bezier(0.1, 0.1, 0.25, 1);
  &::before {
    position: absolute;
    content: "";
    display: block;
    top: 2px;
    left: 7px;
    width: 8px;
    height: 14px;
    border-style: solid;
    border-color: #fff;
    border-width: 0 2px 2px 0;
    transform: rotate(45deg);
    opacity: 0;
  }
  &:checked {
    color: #fff;
    border-color: rgb(255, 0, 106);
    background: rgb(255, 0, 106);
    &::before {
      opacity: 1;
    }
    ~ label::before {
      clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
    }
  }
}

label {
  position: relative;
  cursor: pointer;
  user-select: none;
  &::before {
    position: absolute;
    content: attr(data-content);
    color: rgb(180, 180, 180);
    clip-path: polygon(0 0, 0 0, 0% 100%, 0 100%);
    text-decoration: line-through;
    text-decoration-thickness: 3px;
    text-decoration-color: #252525;
    transition: clip-path 200ms cubic-bezier(0.25, 0.46, 0.45, 0.94);
  }
}

.dark_mode {
  background: #252525;

  h1,
  footer,
  input,
  label {
    color: #fff !important;
  }

  .container {
    background: #252525;
    box-shadow: rgba(255, 255, 255, 0.1) 00px 0px 24px 5px;
  }

  .todo_checkbox {
    border-color: rgb(255, 0, 106);
  }

  label::before {
    color: #fff;
    text-decoration-color: rgb(255, 0, 106);
  }

  footer a:hover {
    color: rgb(255, 0, 106);
  }
}

.toggle {
  --size: 2rem;
  appearance: none;
  outline: none;
  cursor: pointer;
  width: var(--size);
  height: var(--size);
  box-shadow: inset calc(var(--size) * 0.33) calc(var(--size) * -0.25) 0;
  border-radius: 999px;
  color: #fff;
  transition: all 500ms;
  position: absolute;
  top: 20px;
}
.toggle:checked {
  --ray-size: calc(var(--size) * -0.4);
  --offset-orthogonal: calc(var(--size) * 0.65);
  --offset-diagonal: calc(var(--size) * 0.45);
  transform: scale(0.75);
  color: #fa0;
  box-shadow: inset 0 0 0 var(--size),
    calc(var(--offset-orthogonal) * -1) 0 0 var(--ray-size),
    var(--offset-orthogonal) 0 0 var(--ray-size),
    0 calc(var(--offset-orthogonal) * -1) 0 var(--ray-size),
    0 var(--offset-orthogonal) 0 var(--ray-size),
    calc(var(--offset-diagonal) * -1) calc(var(--offset-diagonal) * -1) 0
      var(--ray-size),
    var(--offset-diagonal) var(--offset-diagonal) 0 var(--ray-size),
    calc(var(--offset-diagonal) * -1) var(--offset-diagonal) 0 var(--ray-size),
    var(--offset-diagonal) calc(var(--offset-diagonal) * -1) 0 var(--ray-size);
}

.toggle {
  z-index: 1;
}
.toggle:checked ~ .background {
  --bg: white;
}
</style>