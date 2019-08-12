<template>
    <b-card :border-variant="getCardVariant" @click="$emit('cl', index)">
        {{antwort.antwort}}
        <!--            <img v-if="antwort.image" :src-->
    </b-card>
</template>

<script>
    export default {

        name: "Antwort",
        props: ["antwort", "index", "solution", "selected", "geprueft", "selbsttest"],

        methods: {},
        computed: {


            getCardVariant() {
                if (this.geprueft && this.selbsttest) {
                    if (this.solution.solution[this.index] && this.selected.includes(this.index)) {
                        return "success";
                    }
                    if (this.solution.solution[this.index] && !this.selected.includes(this.index)) {
                        return "warning";
                    }
                    if (!this.solution.solution[this.index] && this.selected.includes(this.index)) {
                        return "danger";
                    }
                } else {
                    if (this.selected.includes(this.index)) {
                        return "primary"
                    }
                }
                return null;
            },


        }
    }
</script>

<style scoped>

</style>