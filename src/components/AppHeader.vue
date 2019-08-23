<template>
    <div class="app-header">
        <b-nav>
            <b-navbar-brand>Quiz App</b-navbar-brand>
            <b-nav-item disabled>
                <strong>Correct :</strong> {{ numCorrect }}/{{
                    numTotal
                }}</b-nav-item
            >
        </b-nav>
    </div>
</template>
<script>
export default {
    name: 'AppHeader',
    props: ['numCorrect', 'numTotal']
};
</script>
<style scoped>
.app-header {
    margin: 30px auto;
}
</style>
