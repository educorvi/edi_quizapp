<template>
    <div>

        <Frage :quizfrage="quizfrage"/>

        <Countdown
                @timeover="$emit('timeover')" class="countdown" id="countdown"
                ref="Countdown"></Countdown>


        <b-card-group deck>


            <Antwort :antwort="antwortComponent"
                     :geprueft="geprueft"
                     :index="index" :key="index"
                     :selbsttest="selbsttest" :selected="selected" :solution="solution" @cl="check"
                     v-for="(antwortComponent, index) in quizfrage.antworten"></Antwort>

        </b-card-group>
        <!--            <b-form-valid-feedback :state="solution.result">Richtig</b-form-valid-feedback>-->
        <!--            <b-form-invalid-feedback :state="solution.result">Falsch</b-form-invalid-feedback>-->
    </div>
</template>

<script>
    import Antwort from "@/components/QuizView/Fragetypen/multibleChoice/Children/Antwort";
    import Frage from "@/components/QuizView/Fragetypen/multibleChoice/Children/Frage";
    import Countdown from "@/components/QuizView/QuizSubIO/CountdownTimer";

    export default {
        data() {
            return {
                selected: [],
            }
        },
        name: "multibleChoice",
        props: ["quizfrage", "geprueft", "solution", "selbsttest", "started"],
        components: {
            Countdown,
            Frage,
            Antwort
        },
        methods: {
            whipe() {
                this.selected = [];
            },
            check(index) {
                if (!this.geprueft) {
                    if (this.selected.includes(index)) {
                        this.selected.splice(this.selected.indexOf(index), 1);
                    } else {
                        this.selected.push(index);
                    }
                    this.$emit('newsel', this.selected)
                }
            }
        },
        mounted() {
            this.$refs.Countdown.startCountdown(this.quizfrage["bedenkzeit"]);
        },
        computed: {
            bedenk() {
                return this.quizfrage["bedenkzeit"];
            }
        },
        watch: {
            bedenk: function (newValue, oldValue) {
                if (newValue !== undefined)
                    this.$refs.Countdown.startCountdown(this.quizfrage["bedenkzeit"]);
            }
        }

    }
</script>

<style>
    /*.card {*/
    /*    margin: 5px;*/
    /*}*/

    .countdown {
        margin-bottom: 8px;
    }
</style>
