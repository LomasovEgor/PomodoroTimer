<template>
    <div class="app">
        <my-header></my-header>
        <PomodoroTimer @pause="timerPause">
        </PomodoroTimer>
        <TaskList :tasks="tasks"
                  @remove="removeTask"
                  @create="createTask"
                  @select="selectTask"
        ></TaskList>
        <my-footer></my-footer>
    </div>
</template>

<script>
import PomodoroTimer from "@/components/PomodoroTimer.vue";
import TaskList from "@/components/TaskList.vue";
import MyFooter from "@/components/MyFooter.vue";
import MyHeader from "@/components/MyHeader.vue";

export default {
    components: {MyHeader, MyFooter, TaskList, PomodoroTimer},
    data() {
        return {
            tasks: this.loadTasks().tasks ? this.loadTasks().tasks : [],
        }
    },
    methods: {
        createTask(task) {
            task.time = 0
            task.isSelected = false
            this.tasks.push(task)
        },
        removeTask(task) {
            this.tasks = this.tasks.filter(p => p.id !== task.id)
        },
        selectTask(task) {
            for (let i = 0; i < this.tasks.length; i++) {
                if ((this.tasks[i].isSelected === true) && (this.tasks[i].id !== task.id)) {
                    this.tasks[i].isSelected = false
                }
                if (this.tasks[i].id === task.id) {
                    this.tasks[i] = task

                }
            }
        },
        timerPause(time) {
            for (let i = 0; i < this.tasks.length; i++) {
                if (this.tasks[i].isSelected === true) {
                    this.tasks[i].time += time
                }
            }
        },
        loadTasks() {
            let tasks = JSON.parse(localStorage.getItem('tasks'))
            return {tasks}
        }
    },
    watch: {
        tasks: {
            handler(newVal) {
                localStorage.setItem('tasks', JSON.stringify(this.tasks));
            },
            deep: true
        }
    }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background: rgb(81, 138, 88);
}

.app {
    font-family: 'Rubik', sans-serif;
    color: white;
}

</style>