<template>
    <div>
        <h1>Editar Tarefa</h1>

        <form action="#" method="POST" @submit.prevent="editTodo">
            <input type="text" name="title" placeholder="Título" v-model="todo.name">
            <input type="text" name="description" placeholder="Descrição" v-model="todo.description">
            <button type="submit" :disabled="todo.loading">
                <span v-if="todo.loading">Aguarde ...</span>
                <span v-else>Enviar</span>
                Enviar
            </button>
        </form>
    </div>
</template>

<script>
import { onMounted, reactive } from 'vue'
import TodoService from '@/services/todos.service'
import router from '@/router'

export default {
    name: 'EditTodo', 

    props: {
        id: {
            require: true
        }
    }, 

    setup(props) {
        
        const todo = reactive({
            name: '', 
            description: '', 
            completed: false, 
            loading: false
        })

        onMounted(async () => {
            todo.loading = true
            TodoService.getTodo(props.id)
                       .then(response => {
                            const todoRes = response.data.data
                            todo.name = todoRes.title
                            todo.description = todoRes.body
                            todo.completed = todoRes.completed == 'S'

                       })
                       .finally(() => todo.loading = false)
        })

        const editTodo = () => {       
            todo.loading = true     
            TodoService.getTodo(props.id)
                       .then(() => router.push({name: 'todos.index'}))
                       .finally(() => todo.loading = false)
        }

        return {
            editTodo, 
            todo
        }
    }
}
</script>