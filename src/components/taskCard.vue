<script>
import { ref } from 'vue';

  export default {
    props: {
        model: {
            required: true,
            default: {
                id: 0,
                title: '',
                description: '',
                status: false
            } 
        }
    },
    emits: {
        onSaveEdit ({ title }) {
            if(title === '') {
                alert('Заполните параметр "Название задачи"!')
                return false
            }
            return true
        }
    },
    setup(props, { emit }) {
        const editing = ref(false);
        const newTitle = ref(props.model.title);
        const newDescription = ref(props.model.description);

        const onDone = () => {
            emit('onDone')
        }

        const onSaveEdit = () => {
            emit('onSaveEdit', { id: props.model.id, newTitle: newTitle.value, newDescription: newDescription.value });
        }

        const onRemove = () => {
            emit('onRemove')
        }

        return {
            onDone,
            onSaveEdit,
            onRemove,
            editing,
            newTitle,
            newDescription
        }
    }
  }
</script>


<template>
    <li>
        <div class="task-card" v-if="!editing">
            <div @click="onDone">
                <h3>{{ model.title }}</h3>
                <p>{{ model.description }}</p>
            </div>
            <div>
                <button @click="editing = true" class="btn-edit">
                    <img src="public\icon-edit.png" alt="редактировать">
                </button>
                <button @click="onRemove" class="btn-red">
                    <img src="public\trash.png" alt="удалить">
                </button>
            </div>
        </div>
        <div class="task-card" v-if="editing">
            <div>
                <input v-model="newTitle" type="text" placeholder="Введите название задачи">
                <input v-model="newDescription" type="text" placeholder="Введите описание задачи">            
            </div>
            <div>
                <button @click="onSaveEdit(), editing = false" class="btn-approve">OK</button>
                <button @click="editing = false" class="btn-red">Отмена</button>
            </div>
        </div>
    </li>
</template>

<style>
li {
    list-style: none;
}

.task-card {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem;
    border-top: 0.15rem solid black;
    margin-top: 1.5rem;
    cursor: pointer;
}

.task-card h3 {
    margin-bottom: 0.5rem;
}

.btn-edit {
    background-color: rgb(167, 167, 167);
    margin-right: 0.5rem;
    margin-bottom: 0.5rem;
}

.btn-red {
    background-color: rgb(243, 82, 82);
}

.btn-approve {
    margin-bottom: 0.5rem;
    background-color: rgb(88, 153, 88);
}

img {
    width: 1rem;
}
</style>