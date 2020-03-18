<template id="task-list">
  <section class="tasks">
    <h1>
      Tasks
      <transition name="fade">
        <small v-if="incomplete">({{ incomplete }})</small>
      </transition>
    </h1>
    <div class="tasks__new input-group">
      <input
        type="text"
        class="input-group-field"
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="New task"
      />
      <span class="input-group-button">
        <button @click="addTask" class="button">
          <i class="fa fa-plus"></i> Add
        </button>
      </span>
    </div>

    <div class="tasks__clear button-group pull-right">
      <button class="button warning small" @click="clearCompleted">
        <i class="fa fa-check"></i> Clear Completed
      </button>
      <button class="button alert small" @click="clearAll">
        <i class="fa fa-trash"></i> Clear All
      </button>
    </div>

    <transition-group name="fade" tag="ul" class="tasks__list no-bullet">
      <task-item
        v-for="(task, index) in tasks"
        @remove="removeTask(index)"
        @complete="completeTask(task)"
        :task="task"
        :key="task"
      ></task-item>
    </transition-group>
  </section>
</template>

<script>
import TaskItem from "./task-item";

export default {
  components: {
    TaskItem
  },
  template: "#task-list",
  props: {
    tasks: { default: [] }
  },
  data() {
    return {
      newTask: ""
    };
  },
  computed: {
    incomplete() {
      return this.tasks.filter(this.inProgress).length;
    }
  },
  methods: {
    addTask() {
      if (this.newTask) {
        this.tasks.push({
          title: this.newTask,
          completed: false
        });
        this.newTask = "";
      }
    },
    completeTask(task) {
      task.completed = !task.completed;
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
    clearCompleted() {
      this.tasks = this.tasks.filter(this.inProgress);
    },
    clearAll() {
      this.tasks = [];
    },

    inProgress(task) {
      return !this.isCompleted(task);
    },
    isCompleted(task) {
      return task.completed;
    }
  }
};
</script>