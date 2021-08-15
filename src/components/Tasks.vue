<template>
  <div>
    <Header>
      <template v-slot:show-hide-list>
        <button
          class="btn-show-hide"
          @click="handleShowHideList"
          v-if="showHideList"
        >
          <span>Ocultar lista</span>
          <img
            src="./../assets/img/eye-icon.svg"
            alt="Ícone de olhos abertos"
          />
        </button>
        <button class="btn-show-hide" @click="handleShowHideList" v-else>
          <span>Ver lista</span>
          <img
            src="./../assets/img/close-eye-icon.svg"
            alt="Ícone de olhos fechados"
          />
        </button>
      </template>
      <template v-slot:add-task>
        <div class="add-task">
          <input
            class="input"
            type="text"
            maxlength="63"
            @keyup.enter="addTask"
            v-model="currentTask"
            v-focus
            ref="input"
          />
          <button class="btn-add-task" @click="addTask">+</button>
        </div>
      </template>
    </Header>
    <main class="main">
      <div class="tasks-list">
        <ul v-if="showHideList">
          <li
            class="task"
            v-for="(task, index) in tasks"
            :key="`${task}-${index}`"
            :class="{ 'line-through': task.isDone }"
          >
            <label class="task-item" :for="`${index}`">
              <input
                class="checkbox"
                type="checkbox"
                :id="`${index}`"
                :checked="task.check"
                @change="complete(task)"
              />
              <CheckboxIcon />
              <span class="task-text">{{ task.name }}</span>
            </label>
            <button class="btn-default" @click="remove(task)">
              <img
                src="./../assets/img/trash-icon.svg"
                alt="Ícone de lixeira com tampa"
              />
            </button>
          </li>
        </ul>
        <p class="task-info" v-else>Lista de tarefas escondida</p>
      </div>
      <div class="deleted-tasks" v-if="deletedTasks.length > 0">
        <p class="task-info">Tarefas excluídas</p>
        <ul>
          <li
            class="task"
            v-for="(task, index) in deletedTasks"
            :key="`${task}-${index}`"
            :class="{ 'line-through': task.isDone }"
          >
            <span class="task-text line-through" style="opacity: .4">{{ task.name }}</span>
            <div class="btns-deleted-task">
              <button class="btn-default" @click="restoreDeletedTasks(task)">
                <img
                  src="./../assets/img/restore-icon.svg"
                  alt="Ícone com duas setas para cima"
                />
              </button>
              <button class="btn-default" @click="removeDeletedTasks(task)">
                <img
                  src="./../assets/img/close-icon.svg"
                  alt="ícone com o símbolo de X"
                />
              </button>
            </div>
          </li>
        </ul>
      </div>
    </main>
  </div>
</template>

<script>
import CheckboxIcon from "@/components/CheckboxIcon.vue";
import Header from "@/components/Header.vue";

const focus = {
  inserted: (el) => {
    // Quando a diretiva é inserida
    el.focus();
    console.log(el);
  },
};

export default {
  directives: { focus },
  name: "Tasks",
  components: { Header, CheckboxIcon },
  data: () => ({
    currentTask: "",
    showHideList: true,
    tasks: [{ name: "Make course", isDone: false, check: false }],
    deletedTasks: [],
  }),
  methods: {
    setFocus() {
      this.$refs.input.focus();
    },
    handleShowHideList() {
      this.showHideList = !this.showHideList;
    },
    addTask() {
      if (this.currentTask.length > 0) {
        this.tasks.push({
          name: this.currentTask,
          isDone: false,
          check: false,
        });
      }
      this.currentTask = "";
      this.saveToStorage();
    },
    remove(task) {
      this.tasks = this.tasks.filter((t) => t.name !== task.name);
      this.showDeletedTasks(task);
      this.saveToStorage();
    },
    complete(task) {
      this.tasks = this.tasks.map((t) => {
        if (t.name === task.name) {
          return { ...t, isDone: !t.isDone, check: !t.check };
        }
        return { ...t };
      });
      this.saveToStorage();
    },
    showDeletedTasks(task) {
      this.sendTasks(this.deletedTasks, task);
      this.saveToStorage();
    },
    restoreDeletedTasks(task) {
      this.sendTasks(this.tasks, task);
      this.deletedTasks = this.deletedTasks.filter((t) => t.name !== task.name);
      this.saveToStorage();
    },
    removeDeletedTasks(task) {
      this.deletedTasks = this.deletedTasks.filter((t) => t.name !== task.name);
      this.saveToStorage();
    },
    saveToStorage() {
      localStorage.setItem("list_tasks", JSON.stringify(this.tasks));
      localStorage.setItem(
        "list_deleted_tasks",
        JSON.stringify(this.deletedTasks)
      );
    },
    sendTasks(tasks, task) {
      tasks.push({
        name: task.name,
        isDone: false,
      });
    },
  },
  mounted() {
    this.setFocus();
  },
  created() {
    this.tasks = JSON.parse(localStorage.getItem("list_tasks")) || [];
    this.deletedTasks =
      JSON.parse(localStorage.getItem("list_deleted_tasks")) || [];
  },
};
</script>

<style scoped>
.tasks-list {
  margin-bottom: var(--space-large);
}

.task {
  display: flex;
  justify-content: space-between;
  column-gap: 1rem;
  background-color: #fff;
  border-radius: 4px;
  padding: 1rem;
  box-shadow: 0 1px 2px var(--shadow-2);
}

.task:not(:last-child) {
  margin-bottom: 6px;
}

.task-item {
  display: flex;
  align-items: center;
  column-gap: var(--space-small);
  cursor: pointer;
}

.task-text {
  color: var(--primary-dark);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 240px;
}

.task-info {
  color: var(--primary-dark);
  font-weight: 700;
  margin-bottom: var(--space-small);
}

.btns-deleted-task {
  display: flex;
  column-gap: var(--space-medium);
}

.btns-deleted-task button:first-child::after {
  content: '';
  background-color: var(--gray);
  width: 1px;
  height: 100%;
  margin-left: var(--space-small);
}

.checkbox {
  position: absolute;
  width: var(--check-circle);
  height: var(--check-circle);
  opacity: 0;
  cursor: pointer;
}

.checkbox:checked ~ .checkbox-icon {
  background-color: var(--tertiary);
}

.checkbox:checked:focus-visible ~ .checkbox-icon {
  box-shadow: var(--focus-shadow);
}

@media (min-width: 768px) {
  .task-text {
    max-width: initial;
  }
}
</style>