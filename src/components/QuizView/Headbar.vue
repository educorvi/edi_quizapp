<template>
    <div>
        <b-navbar class="customNavbar w-100">
            <b-navbar-brand>{{title}}</b-navbar-brand>
            <b-progress class="mt-2" :max="anzahlFragen">
                <b-progress-bar :value="progress.richtig" variant="success"></b-progress-bar>
                <b-progress-bar :value="progress.falsch" variant="danger"></b-progress-bar>
            </b-progress>
        </b-navbar>
    </div>
</template>

<script>
    export default {
        name: "Header",
        props: ["title"]
    }
</script>

<style scoped>

</style>