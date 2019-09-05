<template>
    <div id="app">
        <!--        <Headbar :title="title"/>-->
        <Quizview :baseURL="baseURL"></Quizview>
        <!--        <Sorting :data="test"></Sorting>-->
    </div>
</template>

<script>
    import Quizview from "@/components/QuizView/Quizview";
    import Sorting from "@/components/QuizView/Fragetypen/Sorting/Sorting";

    export default {
        name: 'app',
        components: {
            Sorting,
            Quizview
        },

        data() {
            return {
                baseURL: "https://quiz.educorvi.de/Members/julian--pollinger/testordner",
                title: "Testlernportal",
                test: {
                    labels: [],
                    drops: [
                        "Hallo",
                        "Test",
                        "Lalala",
                        "Tralala",
                        "Damdam"
                    ]
                }
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
