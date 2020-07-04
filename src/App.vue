<template>
    <div id="app">
        <Sudoku/>
    </div>
</template>

<script>
    import Sudoku from './components/Sudoku/Board.vue'

    export default {
        name: 'App',
        components: {
            Sudoku
        }
    }
</script>

<style>
</style>
