<script setup lang="ts">

import { ref, reactive} from 'vue';

const winner = ref<string | null>(null);
const isTie =  ref(false);
const finalGame = ref(false);
const currentPlayer = ref( 'X');




let board = reactive([
    ['','',''],
    ['','',''],
    ['','','']
]);


const checkTie = () => {
    for(let i = 0;i < 3; i++){
        for(let j = 0; j< 3 ; j++){
            if(!board[i][j]){
                return false;
            }
        }
    }
    return true;
}

const checkWin = () => {

    const a = currentPlayer.value;
    for(let i = 0;i < 3; i++){
        //linhas    
        if(board[i].every(cell => cell === a)) return true;
        //colunas
        if(board.every(row => row[i] === a )) return true;
        //valida nas diagonais 
        if(board[0][0] === a && board[1][1] === a && board [2][2] === a ) return true;
        if(board[0][2] === a && board[1][1] === a && board [2][0] === a ) return true;

    }
    return false;
    
}


const playMove = (row: number, col: number) =>{

    if(!board[row][col] && !winner.value){
        board[row][col] = currentPlayer.value;

        if(checkWin()){
            winner.value = currentPlayer.value;
        }

        else if(checkTie()){
                isTie.value = true;
        }
        else{
            currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
        }
    }
} 


const clearTable = () => {
    [
    ['','',''],
    ['','',''],
    ['','','']
];

    currentPlayer.value = 'X';
    finalGame.value = false;
    winner.value = null;
    
}

</script>


<template>
    <div> O jogo vai aqui 



    </div>



</template>


<style>




</style>