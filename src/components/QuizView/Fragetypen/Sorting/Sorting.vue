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
                <b-card :key="element.antwort" class="m-2" v-for="element in quizfrage.antworten">
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
        }
        // mounted() {
        //     let array = [];
        //     for (let i = 0; i < this.quizfrage.antworten.length; i++) {
        //         array[i] = this.quizfrage.antworten[i];
        //     }
        //     for (let i = array.length - 1; i > 0; i--) {
        //         let j = Math.floor(Math.random() * (i + 1));
        //         let temp = array[i];
        //         array[i] = array[j];
        //         array[j] = temp;
        //     }
        //     this.data.drops = array;
        //
        // }
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