<script>
import { toRefs, watch, ref } from 'vue'

export default {
    props: {
        todos: {
            type: Array,
            required: true
        },
        todoEditing: {
            type: null
        }
    },
    setup(props, { emit }) {
        const { todos, todoEditing } = toRefs(props)
        const editingText = ref('')

        const cbFn = (action, id) => {
            switch (action) {
                case 'onDelete':
                    emit('onDelete', id)
                    break;
                case 'onEdit':
                    emit('onEdit', id)
                    break;
                case 'onComplete':
                    emit('onComplete', id)
                    break;
                case 'onUpdate':
                    emit('onUpdate', id, editingText.value)
                    break;
                default:
                    break;
            }
        }

        watch(todoEditing, (todoEditing) => {
            if (todoEditing === null) {
                editingText.value = ''
            }
        })

        return {
            todos,
            todoEditing,
            editingText,
            cbFn
        }
    }
}

</script>

<template>
    <div class="todos">
        <div v-for="({ text, id, completed }, index) in todos" :key="index"
            class="todo">
            <div class="todo-title">
                <input type="checkbox" id="completed" :checked="completed"
                    @change="cbFn('onComplete', id)" />
                <input type="text" v-if="id === todoEditing" v-model.trim="editingText" />
                <div v-else :style="{textDecoration: completed ? 'line-through' : 'auto'}">{{ text }}</div>
            </div>
            <button @click="cbFn('onDelete', id)">Delete</button>
            <button @click="cbFn('onEdit', id)"
                :style="{ display: id === todoEditing ? 'none' : 'block' }">Edit</button>
            <button @click="cbFn('onUpdate', id)"
                :style="{ display: id === todoEditing ? 'block' : 'none' }">Update</button>
        </div>
    </div>
</template>

<style scoped>

</style>
