<template>
    <div>

        <Frage :quizfrage="quizfrage"/>

        <Countdown
                @timeover="$emit('timeover')" class="countdown" id="countdown"
                ref="Countdown"
                v-show="started"></Countdown>

        <b-card-group deck>
            <draggable :disabled="geprueft" class="w-100" en ghost-class="ghost" v-model="quizfrage.antworten">
                <!--                <transition-group name="flip-list" type="transition">-->
                <b-card :border-variant="borderVariant" :key="element.antwort" class="m-2"
                        v-for="element in quizfrage.antworten">
                    {{element.antwort}}
                </b-card>
                <!--                </transition-group>-->
            </draggable>
        </b-card-group>
    </div>
</template>

<script>
    import draggable from "vuedraggable"
    import Frage from "@/components/QuizView/Fragetypen/multibleChoice/Children/Frage";
    import Countdown from "@/components/QuizView/QuizSubIO/CountdownTimer";

    export default {
        name: "Sorting",
        props: ["quizfrage", "started", "solution", "geprueft", "selbsttest"],
        components: {
            Countdown,
            Frage,
            draggable,
        },
        data() {
            return {
                data: {
                    drops: []
                }
            }
        },
        methods: {
            whipe() {
            }
        },
        computed: {
            borderVariant() {
                if (this.geprueft) {
                    if (this.solution.result) {
                        return "success";
                    } else {
                        return "danger";
                    }
                } else {
                    return null;
                }
            },
            bedenk() {
                return this.quizfrage['bedenkzeit'];
            }
        },
        watch: {
            bedenk: function (newValue, oldValue) {
                if (newValue !== undefined)
                    this.$refs.Countdown.startCountdown(this.quizfrage["bedenkzeit"]);
            }
        },
        mounted() {
            this.$refs.Countdown.startCountdown(this.quizfrage["bedenkzeit"]);
        },
    }
</script>

<style scoped>
    .ghost {
        opacity: 0.5;
        background: #c8ebfb;
    }

    .flip-list-move {
        transition: transform 0.5s;
    }
</style>