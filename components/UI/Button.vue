<template>
  <button class="button">
    <slot/>
  </button>
</template>

<style scoped>
.button {
  width: inherit;
  border-radius: 6px;
  color: #FFFFFF;
  border: 1px solid currentColor;
  font-weight: 400;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  position: relative;
  cursor: pointer;
  padding: 10px;

  &:hover {
      opacity: 0.8;
    }

  &[variant="normal"] {
    background: cornflowerblue;
  }

  &[variant="save"] {
    background: darkgreen;
  }

  &[variant="delete"] {
    border-color: transparent;
    background: darkred;
  }

  &[variant="to-do"] {
    border-color: transparent;
    background: salmon;
  }

  &[variant="not-to-do"] {
    border-color: transparent;
    background: black;
  }
}
</style>