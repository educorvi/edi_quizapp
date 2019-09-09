<template>
    <div id="app">
        <!--        <Headbar :title="title"/>-->
        <Quizview :baseURL="baseURL"></Quizview>
        <!--        <Sorting :data="test"></Sorting>-->
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
</style>
