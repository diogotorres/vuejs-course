<template>
  <div class="new-task">
    <input v-model="name" @keydown.enter="add" class="form-element" placeholder="Nova tarefa" type="text">
    <button @click="add" class="form-element">+</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: ''
    }
  },
  methods: {
    add() {
      //emite o evento para o componente pai
      this.$emit('taskAdded', { name: this.name })
      this.name = ''
    }
  }
}
</script>

<style>
  .new-task {
    margin: 35px;
  }

  .form-element {
    outline: none;
    font-size: 2rem;
    border: 1px solid white;
    padding: 5px 10px 8px;
    color: white;
  }

  input.form-element {
    width: 500px;
    background: #FFF2;
    border-top-left-radius: 8px;
    border-bottom-left-radius: 8px;
  }

  button.form-element {
    border-left: none;
    background-color: #2196F3;
    border-top-right-radius: 8px;
    border-bottom-right-radius: 8px;
  }
</style>