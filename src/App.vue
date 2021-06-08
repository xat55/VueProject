<template>
  <div class="container">
    <img :src="logoURL" :alt="logoCaption" width="200" height="200"/>
    <h2>{{ title }}</h2>
    <span>You have {{ allTasks }} {{ allTasks > 1 ? 'tasks' : 'task' }} at the moment</span>


    <div>
      <input type="text" class=""
             v-model="newTask"
             placeholder="Add a new task"
      >
      <button class="btn btn-outline-dark btn-sm"
              @click="addTask"
              :disabled="this.newTask < 1"
      >Add
      </button>
    </div>

    <div v-if="newTask.length > 0">
      <h3>New task preview</h3>
      <p>{{ newTask }}</p>
    </div>

    <ul>
      <li
          v-for="(task, index) in latest"
          :key="task.id"
          @click="finishTask(task)"
          :class="[
                    task.finished ? 'strikeout' : '',
                    task.postponed ? 'text-gray' : '',
                    'simple-class'
          ]"
      >
      {{ index + 1 }}. {{ task.name }}

      <div v-if="task.finished">
        <button @click="deleteTask(latest, index)">Delete task</button>
        <button @click="postponedTask(task)">Postpone task</button>
      </div>
      </li>
    </ul>
  </div>
</template>

<script>

export default {
  data() {
    return {
      title: 'Add a new task',
      newTask: '',
      strikeout: false,
      logoURL: 'https://images.unsplash.com/photo-1507925921958-8a62f3d1a50d?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1955&q=80',
      logoCaption: 'A photo by Kelly Sikkema on Unsplash showing post-it notes',
      tasks: [
        {id: 1, name: 'Learn Vue JS', finished: false, postponed: false},
        {id: 2, name: 'Build a Vue application', finished: false, postponed: false},
        {id: 3, name: 'Write an article about Vue JS', finished: false, postponed: false}
      ]
    }
  },
  methods: {
    addTask() {
      if (this.newTask.length < 1) return

      this.tasks.push({
        id: this.tasks.length + 1,
        name: this.newTask,
        finished: false
      })

      this.newTask = ''
    },
    finishTask(task) {
      task.finished = !task.finished
    },
    deleteTask(latest, index) {
      latest.splice(index, 1)
      this.tasks.length--
    },
    postponedTask(task) {
      task.postponed = !task.postponed
    }
  },

  computed: {
    allTasks() {
      return this.tasks.length
    },
    latest() {
      return [...this.tasks].reverse()
    }
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

li {
  list-style-type: none; /* Убираем маркеры */
}

.strikeout {
  text-decoration: line-through;
}

.text-gray {
  color: gray
}
</style>
