<template>
    <div class="mt-2">
        <b-card no-body v-b-toggle="'collapse_'+frage['@id']">
            <b-card-header :header-text-variant="variant">{{frage.title}}</b-card-header>
            <b-collapse :id="'collapse_'+frage['@id']">
                <b-card-body class="text-left">

                    <!--                    Reihe-->
                    <div v-if="!richtig && frage.antworten[0].bewertung === 'reihe'">
                        <b-row>
                            <b-col class="border-right border-dark">
                                <div class="border-bottom border-dark">Ihre Wahl</div>

                                <div>
                                    <p :key="antwort.antwort+'_f'+index" v-for="(antwort, index) in selected">
                                        {{index+1}}:
                                        {{antwort.antwort}}</p>
                                </div>
                            </b-col>
                            <b-col class="border-left border-dark">
                                <div class="border-bottom border-dark">Richtige Lösung</div>
                                <div>
                                    <p :key="antwort.antwort+'_r'+index" v-for="(antwort, index) in frage.antworten">
                                        {{index+1}}:
                                        {{antwort.antwort}}</p>
                                </div>
                            </b-col>
                        </b-row>
                    </div>
                    <div v-else-if="richtig && frage.antworten[0].bewertung === 'reihe'">
                        <h6>Lösung</h6>
                        <div>
                            <p :key="antwort.antwort+'_r'+index" v-for="(antwort, index) in frage.antworten">
                                {{index+1}}:
                                {{antwort.antwort}}</p>
                        </div>
                    </div>


                    <!--                    Multible Choice-->
                    <div :key="antwort.antwort+'_'+index" v-else v-for="(antwort, index) in frage.antworten">
                        <b-checkbox :checked="selected.includes(index)" :state="getCheckboxState(index)" disabled>
                            <p :style="'color: '+getStyle(index)" class="m-0">{{antwort.antwort}}</p>
                        </b-checkbox>
                    </div>

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
        methods: {
            getCardVariant(index) {
                if (this.frage.antworten[index].bewertung === "richtig" && this.selected.includes(index)) {
                    return "success";
                }
                if (this.frage.antworten[index].bewertung === "richtig" && !this.selected.includes(index)) {
                    return "warning";
                }
                if (this.frage.antworten[index].bewertung === "falsch" && this.selected.includes(index)) {
                    return "danger";
                }

                return null;
            },
            getCheckboxState(index) {
                if (this.frage.antworten[index].bewertung === "richtig" && this.selected.includes(index)) {
                    return true;
                }
                if (this.frage.antworten[index].bewertung === "richtig" && !this.selected.includes(index)) {
                    return false;
                }
                if (this.frage.antworten[index].bewertung === "falsch" && this.selected.includes(index)) {
                    return false;
                }

                return null;
            },
            getStyle(index) {
                if (this.frage.antworten[index].bewertung === "richtig" && this.selected.includes(index)) {
                    return "green";
                }
                if (this.frage.antworten[index].bewertung === "richtig" && !this.selected.includes(index)) {
                    return "red";
                }
                if (this.frage.antworten[index].bewertung === "falsch" && this.selected.includes(index)) {
                    return "red";
                }
                return null;
            }
        }
    }
</script>

<style scoped>

</style>