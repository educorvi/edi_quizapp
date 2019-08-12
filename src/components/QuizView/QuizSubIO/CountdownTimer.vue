<template>
    <div>
        <div id="parent">
            <p :class="getTextClass" v-if="!infinite"><strong>{{countDown}}s</strong></p>
            <p v-else>&infin;</p>
        </div>
    </div>
</template>

<script>

    export default {
        name: "Countdown",
        data() {
            return {
                countDown: 0,
                infinite: true,
                running: true
            }
        },
        methods: {
            countDownTimer() {
                if (this.countDown > 0 && this.running) {
                    setTimeout(() => {
                        this.countDown -= 1;
                        this.countDownTimer()
                    }, 1000);
                } else {
                    if (this.running) {
                        this.$emit("timeover")
                    }
                }
            },
            startCountdown(zahl) {
                this.infinite = true;
                this.countDown = zahl;
                this.running = true;
                if (zahl > 0) {
                    this.infinite = false;
                    this.countDownTimer();
                }
            }
        },
        computed: {
            getTextClass() {
                if (this.countDown <= 10) {
                    return "redText";
                }
                return "blackText"
            }
        }
    }
</script>

<style scoped>
    .blackText {
        color: black;
    }

    .redText {
        color: red;
    }
</style>