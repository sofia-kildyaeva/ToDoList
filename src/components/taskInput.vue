<script>
import { ref } from 'vue'

export default {
    emits: {
        onAddTask({ title }) {
            if (title === '') {
                alert('Заполните параметр "Название задачи"!')
                return false
            }
            return true
        }
    },
    setup(props, { emit }) {
        const title = ref('')
        const description = ref('')

        const onAddTask = () => {
            emit('onAddTask', { title: title.value, description: description.value })
        }

        return {
            title,
            description,
            onAddTask
        }
    }
}
</script>


<template>
    <div class="task-input">
        <input v-model="title" type="text" placeholder="Название задачи">
        <input v-model="description" type="text" placeholder="Описание задачи">
        <button @click="onAddTask" class="btn-add">Добавить</button>
    </div>
</template>


<style>
.task-input {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    margin-top: 0.7rem;
}

input {
    width: 80%;
    height: 2rem;
    margin-top: 1rem;
    padding: 0 0.5rem;
    border-radius: 1rem;
}

.btn-add {
    width: 40%;
    background-color:black;
    margin: 1.5rem 0;
}
</style>