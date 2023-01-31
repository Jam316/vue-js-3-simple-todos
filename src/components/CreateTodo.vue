<script>
import { ref } from 'vue';

export default {
  setup(_, { emit }) {

    const title = ref('')
    const onSubmit = () => {
      const newTodo = {
        id: new Date().getTime(),
        text: title.value,
        completed: false,
      }

      emit('onCreate', newTodo)
      title.value = ''
    }

    return {
      title,
      onSubmit
    }
  }
}



</script>

<template>
    <form @submit.prevent="onSubmit">
      <input type="text" v-model.trim="title" />
      <button type="submit">Add Todo</button>
    </form>
</template>
