<template>
    <div class="container">
        <div class="task-zone">
            <div class="drop-zone" @drop="onDrop($event, 'todo')" @dragenter.prevent @dragover.prevent>
                <h1>To-Do</h1>
                <div class="drag-el" draggable @dragstart="onstart($event, task)" v-for="task in todolist" :key="task.id">{{task.title}}</div>
            </div>
            <div class="drop-zone" @drop="onDrop($event, 'doing')" @dragenter.prevent @dragover.prevent>
                <h1>Doing</h1>
                <div class="drag-el" draggable @dragstart="onstart($event, task)" v-for="task in doinglist" :key="task.id">{{task.title}}</div>
            </div>
            <div class="drop-zone" @drop="onDrop($event, 'done')" @dragenter.prevent @dragover.prevent>
                <h1>Done</h1>
                <div class="drag-el" draggable @dragstart="onstart($event, task)" v-for="task in donelist" :key="task.id">{{task.title}}</div>
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
            ]
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
</style>