<template>
    <div class="container-fluid">

        <div class="row ctlbuttons" id="vorAbgabe" v-show="!geprueft">
            <b-button class="col btn-warning" v-b-modal.modal-hinweis>Hinweis</b-button>
            <b-button @click="$emit('pruefe')" class="col">Prüfen</b-button>
        </div>


        <div class="row ctlbuttons" id="nachAbgabe" v-show="geprueft">
            <b-button class="col btn-danger" v-b-modal.modal-erklaerung>Erklärung</b-button>
            <b-button @click="$emit('naechste')" class="col">Nächste Frage</b-button>
        </div>

        <!--        Hinweis-->
        <b-modal centered id="modal-hinweis" ok-title="Schließen" scrollable title="Hinweis">
            <span v-html="aktuelleFrage.hinweis.data"></span>
            <div class="w-100" slot="modal-footer">
                <b-button @click="$bvModal.hide('modal-hinweis')" block class="mt-3">Schließen</b-button>
            </div>
        </b-modal>


        <!--        Erklärung-->
        <b-modal centered id="modal-erklaerung" ok-title="Schließen" scrollable title="Erklärung">
            <div class="embed-responsive embed-responsive-16by9" style="margin-bottom: 5px"
                 v-if="aktuelleFrage.solutionvideo">
                <span v-html="aktuelleFrage.solutionvideo"></span>
            </div>
            <img :src="aktuelleFrage.solutionimage.download" alt="Erklärungsbild"
                 class="embed-responsive" v-else-if="aktuelleFrage.solutionimage">
            <span v-html="aktuelleFrage.erklaerung.data"></span>
            <div class="w-100" slot="modal-footer">
                <b-button @click="$bvModal.hide('modal-erklaerung')" block class="mt-3">Schließen</b-button>
            </div>
        </b-modal>
    </div>
</template>

<script>
    export default {
        name: "ControlButtons",
        props: ["aktuelleFrage", "geprueft"]
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