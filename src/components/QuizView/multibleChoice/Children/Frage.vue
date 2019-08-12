<template>
    <div class="card shadow" style="margin-bottom: 8px; margin-top: 4px">
        <img :src="quizfrage.image.download" alt="Bild der Fragestellung" class="card-img-top" v-if="quizfrage.image"/>
        <div class="card-header" v-else>
            <h5>{{quizfrage.title}}</h5>
        </div>
        <div class="card-body">
            <h5 v-if="quizfrage.image">{{quizfrage.title}}</h5>
            <p>{{quizfrage.aufgabe}}</p>


            <!--                    Video-->
            <b-modal centered hide-footer id="modal-1" ok-title="Schließen" title="Aufgabenvideo">
                <div class="embed-responsive embed-responsive-16by9">
                    <span v-html="quizfrage.video"></span>
                </div>
                <b-button @click="$bvModal.hide('modal-1')" block class="mt-3">Schließen</b-button>
            </b-modal>

            <div class="row">
                <b-button class="btn-primary col" v-b-modal.modal-1 v-if="quizfrage.video">Video ansehen</b-button>

                <!--            Dateidownload-->
                <b-button :href="quizfrage.datei.download" class="btn-primary col" v-if="quizfrage.datei">
                    {{quizfrage.datei.filename}} herunterladen
                </b-button>
            </div>


        </div>
    </div>
</template>
<script>
    export default {
        name: 'Frage',
        props: {
            quizfrage: {}
        }
    }
</script>
<style scoped>
    iframe {
        width: auto !important;
    }

    button, a {
        margin-bottom: 5px;
        margin-left: 5px;
        margin-right: 5px;
    }

    .card {
        margin-bottom: 6px;
    }
</style>