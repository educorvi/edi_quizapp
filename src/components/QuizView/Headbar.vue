<template>
    <div>
        <b-navbar class="customNavbar w-100" type="light" variant="light">
            <b-navbar-brand>{{title}}</b-navbar-brand>
        </b-navbar>
    </div>
</template>

<script>
    export default {
        name: "Headbar",
        props: ["title"]
    }
</script>

<style scoped>

</style>