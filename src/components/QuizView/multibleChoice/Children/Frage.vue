<template>
    <div class="card shadow-sml">
        <img v-if="quizfrage.image" :src="quizfrage.image.download" class="card-img-top" alt="Titelbild"/>
        <div v-else class="card-header">
            <h5>{{quizfrage.title}}</h5>
        </div>
        <div class="card-body">
            <h5 v-if="quizfrage.image">{{quizfrage.title}}</h5>
            <p>{{quizfrage.aufgabe}}</p>


            <!--                    Video-->
            <b-modal ok-title="Schließen" centered hide-footer id="modal-1" title="Aufgabenvideo">
                <div class="embed-responsive embed-responsive-16by9">
                    <span v-html="quizfrage.video"></span>
                </div>
                <b-button class="mt-3" block @click="$bvModal.hide('modal-1')">Schließen</b-button>
            </b-modal>

            <div class="row">
                <b-button class="btn-primary col" v-if="quizfrage.video" v-b-modal.modal-1>Video ansehen</b-button>

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

    button, a{
        margin-bottom: 5px;
        margin-left: 5px;
        margin-right: 5px;
    }

</style>