<template>
    <div>
        <b-progress :max="anzahlFragen" class="mt-2 rounded-0" height="8px">
            <!--        <b-progress-bar :key="index" :value="1" :variant="getVariant(frage)"-->
            <!--                        v-for="(frage, index) in progress.proFrage"></b-progress-bar>-->
            <b-progress-bar :value="progress.proFrage.length"></b-progress-bar>
        </b-progress>
        <b-progress :max="progress.richtig+progress.falsch" class="rounded-0" height="4px" v-if="selbsttest">
            <b-progress-bar :value="progress.richtig" variant="success"></b-progress-bar>
            <b-progress-bar :value="progress.falsch" variant="danger"></b-progress-bar>
        </b-progress>
    </div>


</template>

<script>
    export default {
        name: "progressIndicator",
        props: ["progress", "anzahlFragen", "selbsttest"],
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