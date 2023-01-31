<script setup lang="ts">
import { ref } from 'vue' 
import Button from './components/Button.vue'

//les lignes de score sont définies 
const scoreLigneHaut = ref(0);
const scoreLigneMilieu = ref(0);
const scoreLigneBas = ref(0);
const scoreColonneGauche = ref(0);
const scoreColonneMilieu = ref(0);
const scoreColonneDroite = ref(0);
const scoreDiagonaleHGBD = ref(0);
const scoreDiagonaleBGHD = ref(0);

//on ajoute tout les scores dans un tableau afin de trouver la valeur de victoire plus facilement
const scores = ref<Number[]>([]);

const turn = ref(false); //false joueur 1 true joueur 2
const joueurScore = ref(0); // la valeur du score sera incrémenté de 1 pour le joueur 1

const jouerVictoire = ref(0);
const gameover = ref(false);

//permet de voir s'il y a un ex eaquo
const nombreDeTour = ref(0);

function changementTour(caseJoue:Number) {
  //on change le tour du joueur 1 ou 2
  turn.value = !turn.value;
  nombreDeTour.value +=1;

  //changement de tour et valeur renseignée pour le tableau des score.
  if(turn.value == false ) {
    joueurScore.value = 1;
  } else {
    joueurScore.value = 7;
  }

  //si joueur 1 obtient 3 sur une des lignes, colonne ou diagonale il gagne, pour le joueur 2 ce sera 21
  //on incrémente toutes les valeurs de chaques ligne.
  switch(caseJoue) {
    case 1: {
      scoreLigneHaut.value += joueurScore.value;
      scoreColonneGauche.value += joueurScore.value;
      scoreDiagonaleHGBD.value += joueurScore.value;
      break;
    }
    case 2: {
      scoreLigneHaut.value += joueurScore.value;
      scoreColonneMilieu.value += joueurScore.value;
      break;
    }
    case 3: {
      scoreLigneHaut.value += joueurScore.value;
      scoreColonneDroite.value += joueurScore.value;
      scoreDiagonaleBGHD.value += joueurScore.value;
      break;
    }
    case 4: {
      scoreColonneGauche.value += joueurScore.value;
      scoreLigneMilieu.value += joueurScore.value;
      break;
    }
    case 5: {
      scoreColonneMilieu.value += joueurScore.value;
      scoreDiagonaleHGBD.value += joueurScore.value;
      scoreDiagonaleBGHD.value += joueurScore.value;
      scoreLigneMilieu.value += joueurScore.value;
      break;
    }
    case 6: {
      scoreColonneDroite.value += joueurScore.value;
      scoreLigneMilieu.value += joueurScore.value;
      break;
    }
    case 7: {
      scoreColonneGauche.value += joueurScore.value;
      scoreLigneBas.value += joueurScore.value;
      scoreDiagonaleBGHD.value += joueurScore.value;
      break;
    }
    case 8: {
      scoreLigneBas.value += joueurScore.value;
      scoreColonneMilieu.value += joueurScore.value;
      break;
    }
    case 9: {
      scoreLigneBas.value += joueurScore.value;
      scoreColonneDroite.value += joueurScore.value;
      scoreDiagonaleHGBD.value += joueurScore.value;
      break;
    }
  }
  //tableau des scores qui nous permets d'avoir toutes les valeurs de j1 ou j2 afin de determiner le gagnant
  scores.value = [scoreLigneHaut.value, scoreLigneMilieu.value, scoreLigneBas.value,scoreColonneGauche.value,scoreColonneMilieu.value,scoreColonneDroite.value,scoreDiagonaleHGBD.value,scoreDiagonaleBGHD.value];

  
  function win(){
    //si on trouve un score egale a 3 dans le tableau alors le joueur X gagne
    if ( scores.value.includes(3) ) {
      jouerVictoire.value = 1;
      console.log("player ❌ win")
    }
    //si on trouve un score egale a 21 dans le tableau alors le joueur Y gagne
    if ( scores.value.includes(21) ) {
      jouerVictoire.value = 2;
      console.log("player 🔵 win")
    }
  
  
    //si un joueur est désigné ou si le nombre de tour est égale a 9 alors c'est la fin de partie
    if(jouerVictoire.value != 0 || nombreDeTour.value > 8) {
      gameover.value = true;
    }
  }

  

  win();
  console.log(turn.value);
  console.log("scores :" + scores.value )
  console.log("scoreLigneHaut     : "+ scoreLigneHaut.value + '\nscoreLigneMilieu   : ' +  scoreLigneMilieu.value + '\nscoreLigneBas      : ' + scoreLigneBas.value + '\nscoreColonneGauche : ' + scoreColonneGauche.value + '\nscoreColonneMilieu : ' + scoreColonneMilieu.value + '\nscoreColonneDroite : ' + scoreColonneDroite.value + '\nscoreDiagonaleHGBD : ' + scoreDiagonaleHGBD.value + '\nscoreDiagonaleBGHD : ' + scoreDiagonaleBGHD.value);
}

function restart(){
    window.location.reload();
    console.log("click !");
  }
</script>

<template>

  <wrapper>
    <header class="flex-item">
   

    <!-- On affiche le tour du joueur soit  🔵 soit ❌ -->
     <div v-if="jouerVictoire>0" > joueur : {{jouerVictoire}} à gagné </div>
     <div> joueur : {{turn ? "🔵" : "❌"}}</div>
     

     

 </header>
 <content class="flex-item">
   <table>
   <tr>
       <td><Button @tour="changementTour(1)" :turn="turn" :disabled="gameover" /></td>
       <td><Button @tour="changementTour(2)" :turn="turn" :disabled="gameover"/></td>
       <td><Button @tour="changementTour(3)" :turn="turn" :disabled="gameover"/></td>
   </tr>
   <tr>
       <td><Button @tour="changementTour(4)" :turn="turn" :disabled="gameover"/></td>
       <td><Button @tour="changementTour(5)" :turn="turn" :disabled="gameover"/></td>
       <td><Button @tour="changementTour(6)" :turn="turn" :disabled="gameover"/></td>
   </tr>
   <tr>
       <td><Button @tour="changementTour(7)" :turn="turn" :disabled="gameover"/></td>
       <td><Button @tour="changementTour(8)" :turn="turn" :disabled="gameover"/></td>
       <td><Button @tour="changementTour(9)" :turn="turn" :disabled="gameover"/></td>
   </tr>
</table>
<br>

<!-- Si la partie est terminé on désactive les boutons et on affiche le bouton nouvelle partie -->
<footer class="flex-item">
  <div v-if="gameover" >
    <button @click="restart()">Nouvelle partie !</button>
  </div>
</footer>


 </content>
  </wrapper>
  

</template>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Rajdhani');

wrapper {
  height: 99%;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.flex-item {

    text-align: center;
}
header {
  font-family: 'Rajdhani';
  font-size: 2rem;

}

body {
  
}

table {
  border-collapse: collapse;
  
 
}
td {
border: 1px solid grey;
height: 150px;
width: 150px;
}

td button {
  font-size: xxx-large;
}

</style>
