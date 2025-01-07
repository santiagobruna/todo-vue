<script setup>
import { reactive } from 'vue';
import Cabecalho from './Cabecalho.vue';
import Formulario from './Formulario.vue';
import ListaDeTarefas from './ListaDeTarefas.vue';
const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    {
      titulo: 'Estudar ES6',
      finalizada: false,
    },
    {
      titulo: 'Estudar SASS',
      finalizada: false,
    },
    {
      titulo: 'Ir para a academia',
      finalizada: true,
    },
  ]
})

const getTarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada === false)
}
const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada === true)
}

const getTarefasFiltradas = () => {
  const { filtro} = estado;

  switch(filtro){
    case 'pendentes':
      return getTarefasPendentes();
    case 'finalizadas':
      return getTarefasFinalizadas(); 
    default:
      return estado.tarefas;  
  }
}

const cadastraTarefa = () => {
  const novaTarefa = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }
  estado.tarefas.push(novaTarefa);
  estado.tarefaTemp = '';
}
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length"/>
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa"/>
    <ListaDeTarefas :tarefas="getTarefasFiltradas()"/>
  </div>
</template>
