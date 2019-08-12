<template>
    <div class="card">
        <div class="card-body">
            <b-form-checkbox :value="index" class="container-fluid">{{antwort.antwort}}</b-form-checkbox>
            <img v-if="antwort.image" :src
        </div>
    </div>
</template>

<script>
    export default {

        name: "Antwort",
        props: ["antwort", "index"]
    }
</script>

<style scoped>

</style>