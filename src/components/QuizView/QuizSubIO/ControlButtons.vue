<template>
    <div class="container-fluid">

        <!--        Für Selbsttests-->
        <div v-if="selbsttest">
            <div class="row ctlbuttons">
                <b-button @click="$emit('vorherige')" class="col" v-if="aktuelleFrage!==0">Vorherige Frage</b-button>
                <b-button @click="$emit('pruefe')" class="col" v-show="!geprueft">Prüfen</b-button>
                <b-button @click="$emit('naechste')" class="col" v-if="aktuelleFrage!==(anzahlFragen-1)"
                          v-show="geprueft">Nächste Frage
                </b-button>
                <b-button @click="$emit('fertig')" class="col" v-else v-show="geprueft">Abschließen</b-button>
            </div>

            <div class="row ctlbuttons">
                <b-button class="col btn-warning" v-b-modal.modal-hinweis>Hinweis</b-button>
                <b-button class="col btn-danger" v-b-modal.modal-erklaerung v-show="geprueft">Erklärung</b-button>
            </div>

        </div>


        <!--        Für Benotete Tests-->
        <div class="row ctlbuttons" v-else>
            <b-button @click="$emit('vorherige')" class="col" v-if="aktuelleFrage!==0">Vorherige Frage</b-button>
            <b-button @click="pruefeUndNaechste" class="col" v-if="aktuelleFrage!==(anzahlFragen-1)">Nächste Frage
            </b-button>
            <b-button @click="$emit('fertig')" class="col" v-else v-show="geprueft">Abschließen</b-button>
            <b-button class="col btn-warning" v-b-modal.modal-hinweis>Hinweis</b-button>
        </div>

        <!--        Hinweis-->
        <b-modal centered id="modal-hinweis" ok-title="Schließen" scrollable title="Hinweis">
            <span v-html="frage.hinweis.data"></span>
            <div class="w-100" slot="modal-footer">
                <b-button @click="$bvModal.hide('modal-hinweis')" block class="mt-3">Schließen</b-button>
            </div>
        </b-modal>


        <!--        Erklärung-->
        <b-modal centered id="modal-erklaerung" ok-title="Schließen" scrollable title="Erklärung">
            <div class="embed-responsive embed-responsive-16by9" style="margin-bottom: 5px"
                 v-if="frage.solutionvideo">
                <span v-html="aktuelleFrage.solutionvideo"></span>
            </div>
            <img :src="frage.solutionimage.download" alt="Erklärungsbild"
                 class="embed-responsive" v-else-if="frage.solutionimage">
            <span v-html="frage.erklaerung.data"></span>
            <div class="w-100" slot="modal-footer">
                <b-button @click="$bvModal.hide('modal-erklaerung')" block class="mt-3">Schließen</b-button>
            </div>
        </b-modal>
    </div>
</template>

<script>
    export default {
        name: "ControlButtons",
        props: ["frage", "geprueft", "selbsttest", "aktuelleFrage", "anzahlFragen"],
        methods: {
            pruefeUndNaechste() {
                this.$emit('pruefe');
                this.$emit('naechste')
            }
        },
        computed: {
            isLetzte() {
                if (this.aktuelleFrage === this.anzahlFragen - 1) {
                    return true;
                }
                return false;
            }
        }
    }
</script>

<style scoped>
    .ctlbuttons {
        margin-left: 6px;
        margin-right: 6px;
    }

    button {
        margin: 5px;
    }
</style>