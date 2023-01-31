<script setup lang="ts">
import { ref,toRefs,defineProps,defineEmits } from 'vue'
const props = defineProps(['turn']);
const emit = defineEmits(['tour']);

const {turn} = toRefs(props); //false j1 true j2
const joueurIcone = ref(0);

function jouer(){

    if(joueurIcone.value == 0) {
        if(turn.value && joueurIcone.value == 0) {
            joueurIcone.value=1;
        
        } else {
            joueurIcone.value = 7;
        }

        emit('tour');
    }
    
}


</script>

<template>
<!-- on emet au parent le changement de tour -->
<button @click="jouer" > 
    <div v-if="joueurIcone==1">🔵  </div>
    <div v-if="joueurIcone==7">❌</div>
    
    {{joueurIcone.value}}
</button>
  
</template>

<style scoped>
button {
  display: block;
  width: 100%;
  height: 100%;
  border: none;
  background-color: #04AA6D;
  padding: 14px 14px;
  font-size: 14px;
  cursor: pointer;
  text-align: center;
}
    
</style>