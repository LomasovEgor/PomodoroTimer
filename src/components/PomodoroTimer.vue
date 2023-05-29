<template>
    <my-dialog v-model:show="dialogVisible"
    >
        <options-form @submit="submitOptions"></options-form>
    </my-dialog>
    <div class="timer">

        <div v-bind:class="{layout_hide: !isPaused}" class="activity__btns">
            <my-button class='activity__btn' @click="setWorkTime">Фокусировка</my-button>
            <my-button class='activity__btn' @click="setShortBreak">Короткий отдых</my-button>
            <my-button class='activity__btn' @click="setLongBreak">Длинный отдых</my-button>
        </div>

        <div class="watch_face">{{ timerTimeStr }}</div>

        <div class="state_info">
            <div id="state" class="info">{{ state }}</div>
            <!--            <div id="turn" class="info"># {{ lapCounter }}</div>-->
        </div>

        <div class="control_buttons">
            <my-button class="control_button" @click="pause">{{ showPause }}</my-button>
            <my-button class="svg_control_button" @click="resetTimer">
                <svg xmlns="http://www.w3.org/2000/svg" height="48" viewBox="0 -960 960 960" width="48">
                    <path d="M480-160q-133 0-226.5-93.5T160-480q0-133 93.5-226.5T480-800q85 0 149 34.5T740-671v-129h60v254H546v-60h168q-38-60-97-97t-137-37q-109 0-184.5 75.5T220-480q0 109 75.5 184.5T480-220q83 0 152-47.5T728-393h62q-29 105-115 169t-195 64Z"/>
                </svg>
            </my-button>
            <!--            <button @click="nextState">Next</button>-->
            <my-button class="svg_control_button" @click="dialogVisible = !dialogVisible">
                <svg xmlns="http://www.w3.org/2000/svg" height="48" viewBox="0 -960 960 960" width="48">
                    <path d="M120-240v-60h720v60H120Zm0-210v-60h720v60H120Zm0-210v-60h720v60H120Z"/>
                </svg>
            </my-button>
        </div>
    </div>
</template>

<script>
import MyButton from "@/components/UI/DefualtButton.vue";
import OptionsForm from "@/components/OptionsForm.vue";
import MyDialog from "@/components/UI/CreateDialog.vue";

export default {
    components: {MyDialog, OptionsForm, MyButton},
    data() {
        return {
            options: {
                workTime: this.loadLocalStorage().workTime ? this.loadLocalStorage().workTime : 25,
                shortBreakTime: this.loadLocalStorage().shortBreakTime ? this.loadLocalStorage().shortBreakTime : 5,
                longBreakTime: this.loadLocalStorage().longBreakTime ? this.loadLocalStorage().longBreakTime : 15,
            },
            state: 'Время сосредоточиться',

            isPaused: true,
            isWorking: true,
            lapCounter: 0,

            timeRemain: (this.loadLocalStorage().workTime ? this.loadLocalStorage().workTime : 25) * 60,
            timePass: 0,
            timerTimeStr: '',

            windowTitle: 'Pomodoro Timer',
            dialogVisible: false
        }
    },

    methods: {
        setWorkTime() {
            this.timeRemain = this.options.workTime * 60
            this.isPaused = true
            this.state = 'Время сосредоточиться'
            this.isWorking = true
            this.timePass = 0
        },
        setShortBreak() {
            this.timeRemain = this.options.shortBreakTime * 60
            this.isPaused = true
            this.state = 'Передышка'
            this.isWorking = false
            this.timePass = 0
        },
        setLongBreak() {
            this.timeRemain = this.options.longBreakTime * 60
            this.isPaused = true
            this.state = 'Время отдохнуть'
            this.isWorking = false
            this.timePass = 0
        },

        resetTimer() {
            this.setWorkTime()
            this.lapCounter = 0
            this.timePass = 0
        },

        async decreaseSec() {
            setInterval(() => {
                if (!this.isPaused) {
                    this.timeRemain--;
                    this.timePass++
                    if (this.timeRemain === 0) {
                        this.timerEnd();
                    }
                }
            }, 1000)
        },
        timerEnd() {
            if (this.isWorking) {
                this.$emit('pause', this.timePass)
                this.setShortBreak()
                this.lapCounter++
            } else {
                this.setShortBreak()
                this.lapCounter++
            }
        },
        getTimerTime() {
            const minutes = Math.floor(this.timeRemain / 60);
            const seconds = this.timeRemain % 60;
            return {min: minutes, sec: seconds};
        },
        pause() {
            this.isPaused = !this.isPaused
            if (this.isWorking) {
                this.$emit('pause', this.timePass)
                this.timePass = 0
            }
        },
        submitOptions(options) {
            this.options = options
            this.dialogVisible = false
            this.resetTimer()
            localStorage.setItem('workTime', String(this.options.workTime))
            localStorage.setItem('shortBreakTime', String(this.options.shortBreakTime))
            localStorage.setItem('longBreakTime', String(this.options.longBreakTime))
        },
        loadLocalStorage() {
            const workTime = localStorage.getItem('workTime');
            const shortBreakTime = localStorage.getItem('shortBreakTime');
            const longBreakTime = localStorage.getItem('longBreakTime');
            return {workTime, shortBreakTime, longBreakTime};
        }
    },

    mounted() {
        this.decreaseSec();
    },

    watch: {
        windowTitle(newValue) {
            document.title = newValue;
        }
    },
    computed: {
        timerTimeStr() {
            const time = this.getTimerTime()
            const minutes = time.min
            const seconds = time.sec
            return `${minutes < 10 ? '0' + minutes : minutes}:${seconds < 10 ? '0' + seconds : seconds}`;
        },
        windowTitle() {
            let ret = ''
            if (this.isPaused) {
                ret += 'Paused - '
            } else {
                if (this.getTimerTime.min < 1)
                    ret += (this.getTimerTime.sec + 's - ');
                else
                    ret += (this.getTimerTime.min + 'min - ');
            }
            return ret
        },
        showPause() {
            if (this.isPaused) {
                return "Старт"
            } else {
                return "Пауза"
            }
        }
    }
}


</script>

<style scoped>
.timer {
    display: flex;
    justify-content: center;
    flex-direction: column;
    margin-top: 1rem;
    font-weight: bold;
    text-align: center;
}

.activity__btns {
    display: flex;
    justify-content: center;
}

.activity__btn {
    margin-left: 10px;
}

.control_buttons {
    display: flex;
    justify-content: center;
    margin-top: 1rem;
}

.control_button {
    margin-left: 10px;
    margin-top: 10px;
    min-width: 200px;
}


svg {
    max-width: 40px;
    max-height: 40px;
    fill: rgb(81, 138, 88)
}

.svg_control_button {
    margin-left: 10px;
    padding: 0;
    margin-top: 10px;
}

.watch_face {
    font-size: 10rem;
    margin-top: 0;
    padding: 0;
}

.state_info {
    font-size: 2rem;
}

.layout_hide {
    opacity: 0;
}


@media (max-width: 480px) {

    .activity__btns {
        display: flex;
        flex-direction: column;
        width: 90%;
        margin: auto;
    }

    .activity__btn {
        margin-top: 5px;
        margin-left: 0;
    }

    .control_buttons {
        display: flex;
        flex-direction: column;
        width: 90%;
        margin: auto;
    }

    .control_button {
        min-height: 49px;
        margin-top: 5px;
        margin-left: 0;
    }

    .svg_control_button {
        margin-left: 0;
        padding: 0;
        margin-top: 5px;
    }

    .watch_face {
        font-size: 8rem;
    }

}

@media (max-width: 355px) {
    .watch_face {
        font-size: 6rem;
    }

    .state_info {
        font-size: 1.5rem;
    }
}
</style>