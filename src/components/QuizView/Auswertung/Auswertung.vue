<template>
    <div class="text-center">
        <h4 class="mb-2">Auswertung</h4>

        <Details
                :frage="frage"
                :key="index"
                :richtig="proFrage[index]"
                :selected="selected[index]"
                v-for="(frage, index) in fragen"
        >
        </Details>
    </div>
</template>

<script>
    import Details from "@/components/QuizView/Auswertung/Details";
    import axios from "axios";

    export default {
        name: "Auswertung",
        components: {Details},
        props: ["proFrage", "baseURL", "selected", "richtig", "falsch"],
        data() {
            return {
                fragen: [],
            }
        },
        mounted() {
            axios.get(this.baseURL + "?fullobjects=1", {headers: {Accept: "application/json"}}).then(res => this.fragen = res.data.items);
        },
        computed: {
            // series() {
            //     return [this.richtig, this.falsch];
            // }
        },
    }
</script>

<style scoped>

</style>