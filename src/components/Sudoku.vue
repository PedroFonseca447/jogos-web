<script setup lang="ts">

    import { reactive, ref } from 'vue';



    const selectRow = ref<number | null>(null)
    const selectCol = ref<number | null>(null)
    const originalBoard = reactive<number[][]>([])
    const  board = reactive<number[][]>([])
    const victory = ref<boolean | null>(null)


    //vicular filledCells a dificuldades: ai faz um calculo depois de quantas aleatorias me servem

    function generateBoard( filledCells = 25): number[][] {
        const board = Array.from({length: 9}, () => Array(9).fill(0))
        let count = 0;


        while( count < filledCells){

            const row = Math.floor(Math.random() *9)
            const col = Math.floor(Math.random() * 9)
            const num = Math.floor(Math.random() * 9) + 1 //aqui vai ser multilicando o valor para nao dar =0 

            if(board[row][col] === 0){
                board[row][col] = num; //preenche a posicao
                count ++;
            }
        }

        return board;
    }


</script>


<template>
    <div></div>
</template>

<style>


</style>