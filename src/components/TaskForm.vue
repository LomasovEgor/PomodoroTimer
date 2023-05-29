<template>
    <div class="set_options_form">
        <form @submit.prevent>
            <h4>Создать задачу</h4>
            <div class="inputs">
                <my-input
                        v-model="task.title"
                        type="text"
                        placeholder="Название"/>
                <my-input
                        v-model="task.body"
                        type="text"
                        placeholder="Описание"/>
            </div>
            <my-button
                    style="align-self: flex-end; margin-top: 15px;"
                    @click="createPost"

            >
                Создать
            </my-button>

        </form>
    </div>

</template>

<script>

import MyButton from "@/components/UI/DefualtButton.vue";
import MyInput from "@/components/UI/MyInput.vue";

export default {
    components: {MyInput, MyButton},
    data() {
        return {
            task: {
                title: '',
                body: ''
            }
        }
    },
    methods: {
        createPost() {
            this.task.id = Date.now()
            console.log(this.task)
            this.$emit('create', this.task);
            this.task = {
                title: '',
                body: ''
            }
        },
    }
}

</script>

<style scoped>

form {
    display: flex;
    flex-direction: column;
}

.set_options_form {
    background: rgb(81, 138, 88);
}

h4 {
    color: white;
    font-size: 1.5em;
}

.inputs {
    margin-top: 10px;
}
</style>