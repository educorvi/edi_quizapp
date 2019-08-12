<template>
    <div>
        <p v-if="!infinite">{{countDown}}s</p>
        <h6 v-else>&infin; s</h6>
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
                    this.infinite = false;
                    setTimeout(() => {
                        this.countDown -= 1;
                        this.countDownTimer()
                    }, 1000);
                } else {
                    if (!this.infinite) {
                        this.$emit("timeover")
                    }
                    this.countDownTimer();
                }
            },
        },
        created() {

            this.countDown = this.duration;
            this.countDownTimer()
        },
        props: ["duration"]
    }
</script>

<style scoped>

</style>