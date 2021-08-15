<template>
  <div>
    <Tasks />
  </div>
</template>

<script>
import Tasks from "./components/Tasks.vue";

export default {
  components: { Tasks },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Nunito:wght@400;700&display=swap");

:root {
  --check-circle: 0.875rem;
  --light-gray: #f7f7f7;
  --gray: #ccc;
  --primary-light: #dff9f9;
  --primary: 45deg, #21D4FD 0%, #B721FF 100%;
  --primary-dark: #0b1e51;
  --secondary: #B721FF;
  --tertiary: #21D4FD;
  --light-color: rgba(255, 255, 255, 0.7);
  --shadow-2: rgba(0, 0, 0, 0.2);
  --space-small: 0.5rem;
  --space-medium: 1rem;
  --space-large: 2rem;
  --focus-shadow: 0 0 0 1px white, 0 0 0 2px var(--secondary);
}

body {
  background-color: var(--light-gray);
  font-family: "Nunito", sans-serif;
  font-weight: 400;
}

.main {
  padding: var(--space-medium);
}

* {
  margin: 0;
  padding: 0;
}

.line-through {
  text-decoration: line-through;
}

.line-through span {
  user-select: none;
}

.btn-default {
  display: flex;
  align-items: center;
  column-gap: var(--space-small);
  background: none;
  color: var(--primary-dark);
  border: none;
  position: relative;
  z-index: 1;
  cursor: pointer;
}

.btn-default:focus-visible {
  outline: none;
}

.btn-default:focus-visible img {
  box-shadow: var(--focus-shadow);
}

.btn-default:hover::before,
.btn-default:active::before {
  content: "";
  width: var(--space-medium);
  height: var(--space-medium);
  background-color: #ececec;
  padding: 0.5rem;
  position: absolute;
  left: -0.5rem;
  border-radius: 50%;
  z-index: -1;
}
</style>
