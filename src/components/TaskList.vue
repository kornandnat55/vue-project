<template>
    <div class="container">
        <div class="add-task">
            <input id="new-task" type="text" v-model="newTask">
            <button type="botton" @click="addTask(newTask)">Add New Task</button>
        </div>
        <div class="task-zone">
            <div class="drop-zone" @drop="onDrop($event, 'todo')" @dragenter.prevent @dragover.prevent>
                <h1>To-Do</h1>
                <div class="drag-el" draggable @dragstart="onstart($event, task)" v-for="task in todolist" :key="task.id">
                    <span v-if="editTask != task.id">{{task.title}}</span>
                    <input v-else class="edit-task" type="text" v-model="task.title">
                    <button v-if="editTask != task.id" type="button" @click="onEdit(task)">Edit</button>
                    <button v-else type="button" @click="editedTask(task)">Save</button>
                    <button type="button" @click="deleteTask(task)">Delete</button>
                </div>
            </div>
            <div class="drop-zone" @drop="onDrop($event, 'doing')" @dragenter.prevent @dragover.prevent>
                <h1>Doing</h1>
                <div class="drag-el" draggable @dragstart="onstart($event, task)" v-for="task in doinglist" :key="task.id">
                    <span v-if="editTask != task.id">{{task.title}}</span>
                    <input v-else class="edit-task" type="text" v-model="task.title">
                    <button v-if="editTask != task.id" type="button" @click="onEdit(task)">Edit</button>
                    <button v-else type="button" @click="editedTask(task)">Save</button>
                    <button type="button" @click="deleteTask(task)">Delete</button>
                </div>
            </div>
            <div class="drop-zone" @drop="onDrop($event, 'done')" @dragenter.prevent @dragover.prevent>
                <h1>Done</h1>
                <div class="drag-el" draggable @dragstart="onstart($event, task)" v-for="task in donelist" :key="task.id">
                    <span v-if="editTask != task.id">{{task.title}}</span>
                    <input v-else class="edit-task" type="text" v-model="task.title">
                    <button v-if="editTask != task.id" type="button" @click="onEdit(task)">Edit</button>
                    <button v-else type="button" @click="editedTask(task)">Save</button>
                    <button type="button" @click="deleteTask(task)">Delete</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TaskList',
    data(){
        return{
            tasks:[
                {
                    id: 1,
                    title: 'Item A',
                    status: 'todo'
                },
                {
                    id: 2,
                    title: 'Item B',
                    status: 'todo'
                },
                {
                    id: 3,
                    title: 'Item C',
                    status: 'todo'
                },
                {
                    id: 4,
                    title: 'Item D',
                    status: 'doing'
                }
            ],
            newTask: "",
            editTask: ""
        }
    },
    computed:{
        todolist(){
            return this.tasks.filter(task => task.status == "todo")
        },
        doinglist(){
            return this.tasks.filter(task => task.status == "doing")
        },
        donelist(){
            return this.tasks.filter(task => task.status == "done")
        }
    },
    methods:{
        onstart(e, task){
            e.dataTransfer.dropEffect = "move"
            e.dataTransfer.effectAllowed = "move"
            e.dataTransfer.setData('taskId', task.id)
        },
        onDrop(e, newstatus){
            const taskId = e.dataTransfer.getData('taskId')
            const task = this.tasks.find(task => task.id == taskId)
            task.status = newstatus
        },
        addTask(newTask){
            let newId = this.tasks.length + 1
            this.tasks.push({ id: newId, title: newTask, status: 'todo'})
            this.newTask = ""
        },
        onEdit(task){
            this.editTask = task.id
        },
        editedTask(updateTask){
            const task = this.tasks.find(task => task.id == updateTask.id)
            task.title = updateTask.title
            this.editTask = ""
        },
        deleteTask(deleteTask){
           this.tasks = this.tasks.filter(task => task.id != deleteTask.id)
        }
    }
}
</script>

<style scoped>
    .container{
        margin: 'TaskList';
    }
    .task-zone{
        display: flex;
        justify-content: space-around;
    }
    .drop-zone{
        border: 1px solid black;
        width: 250px;
        height: 400px;
        padding: 10px ;
    }
    .drag-el{
        border: 1px solid black;
    }
    .add-task{
        margin: 30px 0;
    }
</style>