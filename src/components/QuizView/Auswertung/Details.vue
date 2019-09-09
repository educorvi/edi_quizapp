<template>
    <div class="mt-2">
        <b-card no-body v-b-toggle="'collapse_'+frage['@id']">
            <b-card-header :header-text-variant="variant">{{frage.title}}</b-card-header>
            <b-collapse :id="'collapse_'+frage['@id']">
                <b-card-body>
                    <b-row>
                        <b-col class="border-right border-dark">
                            <div class="border-bottom border-dark">Ihre Wahl</div>

                            <div v-if="frage.antworten[0].bewertung === 'reihe'">
                                <p :key="antwort.antwort" v-for="(antwort, index) in selected">{{index+1}}:
                                    {{antwort.antwort}}</p>
                            </div>
                            <div v-else>
                                <p :key="frage.antworten[antwort].antwort" v-for="antwort in selected">
                                    {{frage.antworten[antwort].antwort}}</p>
                            </div>
                        </b-col>
                        <b-col class="border-left border-dark">
                            <div class="border-bottom border-dark">Richtige Lösung</div>

                            <div v-if="frage.antworten[0].bewertung === 'reihe'">
                                <p :key="antwort.antwort" v-for="(antwort, index) in frage.antworten">{{index+1}}:
                                    {{antwort.antwort}}</p>
                            </div>
                            <div v-else>
                                <div :key="antwort.antwort" v-for="antwort in frage.antworten">
                                    <p v-if="antwort.bewertung==='richtig'">{{antwort.antwort}}</p>
                                </div>
                            </div>
                        </b-col>
                    </b-row>
                </b-card-body>
            </b-collapse>
        </b-card>
    </div>
</template>

<script>
    export default {
        name: "Details",
        props: ["richtig", "selected", "frage"],
        computed: {
            variant() {
                if (this.richtig) {
                    return "success";
                } else {
                    return "danger";
                }
            },
        },
    }
</script>

<style scoped>

</style>