<template>
    <div class="task_list">
        <my-dialog v-model:show="dialogVisible"
        >
            <PostForm @create="createTask"></PostForm>
        </my-dialog>
        <h3 class="tasks_title">Задачи:</h3>
        <hr>
        <div v-if="tasks.length > 0">
            <task-item
                    v-for="task in tasks"
                    :task="task"
                    :key="task.id"
                    @remove="$emit('remove', task)"
                    @select="selectTask"
            >
            </task-item>
        </div>
        <h3>
            <my-button class="create_task__btn"
                       @click="dialogVisible = true"
            >
                Создать задачу
            </my-button>
        </h3>
    </div>

</template>

<script>
import TaskItem from "@/components/TaskItem.vue";
import MyDialog from "@/components/UI/CreateDialog.vue";
import PostForm from "@/components/TaskForm.vue";
import MyButton from "@/components/UI/DefualtButton.vue";

export default {
    components: {MyButton, PostForm, MyDialog, TaskItem},
    data() {
        return {
            dialogVisible: false
        }
    },
    props: {
        tasks: {
            type: Array,
            required: true
        }
    },
    methods: {
        createTask(task) {
            this.$emit('create', task)
            this.dialogVisible = false

        },
        selectTask(task) {
            this.$emit('select', task)
        }
    }
}
</script>

<style scoped>

.task_list {
    max-width: 480px;
    padding: 20px 20px 0 20px;
    margin: 20px auto 42px;
}

.create_task__btn {
    margin-top: 15px;
}

.tasks_title {
    font-size: 1.5rem;
}

@media (max-width: 480px) {

    .create_task__btn {
        display: flex;
    }
}
</style>