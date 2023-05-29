<template>
    <div class="task"
         v-bind:class="{task_active: task.isSelected}"
         @click="selectTask"
    >
        <div>
            <div v-if="task.title.length > 0"><strong>Название:</strong> {{ task.title }}</div>
            <div v-if="task.body.length > 0"><strong>Описание:</strong> {{ task.body }}</div>
            <div><strong>Время фокуса:</strong> {{ TimeToStr }}</div>
        </div>
        <my-button class="task__btn"
                   @click="removeTask"
        >
            Удалить
        </my-button>
    </div>
</template>

<script>

import MyButton from "@/components/UI/DefualtButton.vue";

export default {
    components: {MyButton},
    props: {
        task: {
            type: Object,
            required: true
        },
    },
    methods: {
        removeTask() {
            this.$emit('remove', this.task)
        },
        selectTask() {
            this.task.isSelected = !this.task.isSelected
            this.$emit('select', this.task)
        }
    },
    computed: {
        TimeToStr() {
            let seconds = this.task.time
            let hours = Math.floor(seconds / 3600);
            let minutes = Math.floor((seconds - hours * 3600) / 60);
            let remainingSeconds = seconds % 60;

            let result = "";
            if (hours > 0) {
                result += hours + " ч ";
            }
            if (minutes > 0) {
                result += minutes + " мин ";
            }
            if (remainingSeconds > 0) {
                result += remainingSeconds + " сек";
            }
            if (result === "") {
                result = "0 сек";
            }

            return result;
        }
    }
}
</script>

<style scoped>
.task {
    margin-top: 15px;
    padding: 15px;
    display: flex;
    align-items: center;
    justify-content: space-between;

    border: 3px solid white;
    border-radius: 8px;
    background: white;
    color: #2C3D2FFF;
}

.task:hover {
    border: 3px solid rgb(81, 138, 88);
}

.task_active {
    margin-top: 15px;
    padding: 15px;
    border: 3px solid #6fca62;
    display: flex;
    align-items: center;
    justify-content: space-between;
    background: #6fca62;
    color: #2C3D2FFF;
}
</style>