<template>
  <header class="header">
    <div class="header-content">
      <div class="wrapper-logo">
        <div class="logo">
          <img
            src="./../assets/img/logo.svg"
            alt="Ícone de confirmação de tarefas"
          />
          <span>MyTasks</span>
        </div>
        <slot name="show-hide-list"></slot>
      </div>
      <div class="input-wrapper">
        <p class="input-legend">Digite a tarefa e adicione</p>
        <slot name="add-task"></slot>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {
      show: true,
    };
  },
};
</script>

<style>
.header {
  background-color: #fff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  position: sticky;
  top: 0;
  z-index: 2;
}

.header-content {
  padding: var(--space-medium);
}

header::before {
  content: "";
  display: block;
  width: 100%;
  height: var(--space-medium);
  background-color: #21D4FD;
  background-image: linear-gradient(var(--primary));
}

.wrapper-logo {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: var(--space-small);
}

.logo {
  display: flex;
  column-gap: 0.25rem;
  color: var(--primary-dark);
  font-weight: 700;
  font-size: 1.25rem;
}

.input-wrapper {
  position: relative;
}

.input {
  background: transparent;
  color: var(--primary-dark);
  border: none;
  border-bottom: 2px solid var(--primary-dark);
  padding-bottom: 1px;
  font-size: 1rem;
  flex: 1;
}

.input-legend {
  font-size: 0.875rem;
  color: var(--primary-dark);
  margin-bottom: 2px;
}

input:focus-visible {
  outline: none;
}

.add-task {
  display: flex;
  align-items: center;
  justify-content: space-between;
  column-gap: 1rem;
  margin-top: -0.5rem;
}

.btn-show-hide {
  display: flex;
  align-items: center;
  column-gap: var(--space-small);
  background: none;
  border: none;
  padding: 2px;
  border-radius: 4px;
}

.btn-show-hide:focus-visible {
  box-shadow: var(--focus-shadow);
  outline: none;
}

.btn-show-hide:active {
  background-color: #ececec;
}

.btn-show-hide:hover::before,
.btn-show-hide:active::before {
  content: none;
}

.btn-add-task {
  display: grid;
  place-content: center;
  font-size: 2rem;
  background-image: linear-gradient(var(--primary));
  color: var(--light-color);
  width: 2rem;
  height: 2rem;
  padding: 0.2rem;
  border-radius: 50%;
  border: none;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
  transition: 0.2s;
}

.btn-add-task:focus-visible {
  box-shadow: var(--focus-shadow);
  outline: none;
} 

.btn-add-task:hover,
.btn-add-task:active {
  opacity: 0.7;
  transform: scale(1.1);
}
</style>