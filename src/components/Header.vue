<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled><b>Quiz App</b></b-nav-item>
            <b-nav-item disabled>Counter:{{numCorrect}}/{{numTotal}}</b-nav-item>
            <b-nav-item disabled>Score: {{score}} %</b-nav-item>
            
        </b-nav>
    </div>
</template>

<script>
export default {
    props: [
        'numCorrect',
        'numTotal',
        'score'
    ]
}
</script>

<style scoped>
* {
    font-size: 20px;
}
</style>