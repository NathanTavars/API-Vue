<template>
    <div>
        <input type="checkbox" :checked="completed" @change="toogleStatus">
        {{ todo.title }} - {{ todo.body}}
        <router-link :to="{name: 'todos.edit', params: {id: todo.identify}}"><i class="fas fa-edit"></i></router-link>
        <a href="#" @click.prevent="deleteTodo"><i class="fas fa-trash-alt"></i></a>
    </div>
</template>

<script>
import TodoService from '@/services/todos.services';
import { computed } from 'vue';

    export default {
        name: 'TodoSingle',
        props: {
            todo: {
                require: true,
                type: Object,
            }
        },
        setup(props, {emit}) {
            const completed = computed(() => props.todo.completed == 'S')

            const deleteTodo = () => {
                TodoService.deleteTodo(props.todo.identify)
                    .then(() => emit('todoDeleted', props.todo))
            }

            const toogleStatus = () => {
                const todo = props.todo
                const params = {
                    name: todo.title,
                    description: todo.body,
                    completed: !completed.value
                }

                TodoService.editTodo(props.todo.identify, params)
                .then(() => emit('todoUpdate', params))
            }
            

            return {
                deleteTodo,
                completed,
                toogleStatus,
            }
            
        }
    }

</script>