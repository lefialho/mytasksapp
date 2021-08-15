<template>
  <svg
    class="checkbox-icon"
    xmlns="http://www.w3.org/2000/svg"
    viewBox="0 0 22 22"
  >
    <path
      class="tick"
      stroke="rgba(255,255,255, .7)"
      fill="none"
      stroke-linecap="round"
      stroke-width="4"
      d="M4 10l5 5 9-9"
    ></path>
  </svg>
</template>

<script>
export default {
  name: "CheckboxIcon",
};
</script>

<style scoped>
.checkbox-icon {
  width: var(--check-circle);
  height: var(--check-circle);
  background-color: transparent;
  border: 1px solid var(--tertiary);
  border-radius: 50%;
  padding: .2rem;
}

.tick {
  stroke-dasharray: 20px;
  stroke-dashoffset: 20px;
  transition: stroke-dashoffset 0.2s ease-out;
}

.checkbox:checked + .checkbox-icon .tick {
  stroke-dashoffset: 0;
}
</style>