<template>
    <!--    <b-progress class="mt-2" :max="anzahlFragen">-->
    <!--        <b-progress-bar :value="progress.richtig" variant="success"></b-progress-bar>-->
    <!--        <b-progress-bar :value="progress.falsch" variant="danger"></b-progress-bar>-->
    <!--    </b-progress>-->

    <b-progress :max="anzahlFragen" class="mt-2">
        <b-progress-bar :key="index" :value="1" :variant="getVariant(frage)"
                        v-for="(frage, index) in progress.proFrage"></b-progress-bar>
    </b-progress>
</template>

<script>
    export default {
        name: "progressIndicator",
        props: ["progress", "anzahlFragen"],
        methods: {
            getVariant(richt) {
                if (richt) {
                    return "success";
                } else {
                    return "danger"
                }
            }
        }
    }
</script>

<style scoped>

</style>