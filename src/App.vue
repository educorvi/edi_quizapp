<template>
    <div id="app">
        <!--        <Headbar :title="title"/>-->
        <Quizview :baseURL="baseURL+'/Members/julian--pollinger/testordner'"></Quizview>
    </div>
</template>

<script>
    import Quizview from "@/components/QuizView/Quizview";

    export default {
        name: 'app',
        components: {
            Quizview
        },

        data() {
            return {
                baseURL: "http://192.168.86.52:8080/quiz",
                title: "Testlernportal"
            }
        },


    }
</script>

<style>
    /*#app {*/
    /*  font-family: 'Avenir', Helvetica, Arial, sans-serif;*/
    /*  -webkit-font-smoothing: antialiased;*/
    /*  -moz-osx-font-smoothing: grayscale;*/
    /*  text-align: center;*/
    /*  color: #2c3e50;*/
    /*  margin-top: 60px;*/
    /*}*/
</style>
