<template>
    <div id="app">
        <b-container>
            <AppHeader />
            <QuizBox
                v-if="questions.length"
                :currentQuestion="questions[index]"
                :next ="next"
            />
        </b-container>
    </div>
</template>

<script>
import AppHeader from './components/AppHeader';
import QuizBox from './components/QuizBox';
export default {
    name: 'app',
    components: {
        AppHeader,
        QuizBox
    },
    data() {
        return {
            questions: [],
            index:0,
        };
    },
    methods:{
        next:function () {
            this.index++;
        }
    },
    mounted: function() {
        fetch(
            'https://opentdb.com/api.php?amount=10&category=32&type=multiple',
            { method: 'get' }
        )
            .then(response => response.json())
            .then(jsonData => {
                this.questions = jsonData.results;
            });
    }
};
</script>

<style>

</style>
