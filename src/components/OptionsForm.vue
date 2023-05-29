<template>
    <div class="set_options_form">
        <form @submit.prevent>
            <h4>Настройки интервалов</h4>
            <div class="inputs">
                <div class="number_input">
                    <div class="input_title">Фокусировка</div>
                    <my-input
                            v-model="options.workTime"
                            type="number"
                            placeholder="25"/>
                </div>
                <div class="number_input">
                    <div class="input_title">Короткий отдых</div>
                    <my-input
                            v-model="options.shortBreakTime"
                            type="number"
                            placeholder="5"/>
                </div>
                <div class="number_input">
                    <div class="input_title">Длинный отдых</div>
                    <my-input
                            v-model="options.longBreakTime"
                            type="number"
                            placeholder="15"/>
                </div>

            </div>
            <my-button
                    style="align-self: flex-end; margin-top: 15px;"
                    @click="submit"
            >
                <strong>ОК</strong>
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
            options: {
                workTime: 25,
                shortBreakTime: 5,
                longBreakTime: 15,
            },
        }
    },
    methods: {
        submit() {
            this.$emit('submit', this.options);
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
    display: flex;
    margin-top: 10px;
    max-width: 320px;
}

.number_input {
    margin-left: 5px;
}

.input_title {
    font-size: 12px;
}

@media (max-width: 355px) {
    .input_title {
        font-size: 12px;
        min-height: 30px;
    }
}

</style>