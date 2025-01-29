<template>
<div>
    <section class="score">
        Player <span>{{ this.winCount }}</span> x <span>{{ this.loseCount }}</span> Computer
    </section>
    <hr>
</div>



</template>
<script>
    export default {
        props: ['winCount', 'loseCount']

    }

</script>

<style lang="scss">
    section.score span {
        border: 1px solid gray;
        padding: 3px;
        border-radius: 5px;
    }
</style>