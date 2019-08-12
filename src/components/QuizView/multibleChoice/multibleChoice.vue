<template>
    <div class="container-fluid">

        <Frage :quizfrage="quizfrage"/>

        <b-form-group>
            <b-checkbox-group v-model="selected">
                <Antwort v-for="(antwortComponent, index) in quizfrage.antworten" v-bind:key="index"
                         v-bind:index="index" v-bind:antwort="antwortComponent"></Antwort>
            </b-checkbox-group>
        </b-form-group>
    </div>
</template>

<script>
    import Antwort from "@/components/QuizView/Quizfrage/Children/Antwort";
    import Frage from "@/components/QuizView/Quizfrage/Children/Frage";

    export default {
        data() {
            return {
                selected: []
            }
        },
        name: "Quizfrage",
        props: ["quizfrage"],
        components: {
            Frage,
            Antwort
        }
    }
</script>

<style>
    .card {
        margin: 8px;
    }
</style>
