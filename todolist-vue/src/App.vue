<script setup>
import { reactive } from 'vue';
import Header from './components/Header.vue'
import Forms from './components/Forms.vue'
import List from './components/List.vue'

const estado = reactive({
  filtro: 'todas',
  tarefas: [
    {
      titulo: 'Estudar',
      finalizada: false,
    }
  ],
  tarefaTemp: '',
})

const tarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}

const tarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const filtroTarefas = () => {
  const { filtro } = estado

  switch(filtro) {
    case 'pendentes':
      return tarefasPendentes()
    case 'finalizadas':
      return tarefasFinalizadas()
    default:
      return estado.tarefas
  }
  
}

const cadastraTarefa = () => {
  const novaTarefa = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }
  estado.tarefas.push(novaTarefa)
  estado.tarefaTemp = ''
}
</script>

<template>
<div class="container">
  <Header :tarefas-pendentes="tarefasPendentes().length" />
  <Forms :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa" :trocar-filtro="evento => estado.filtro = evento.target.value" />
  <List :tarefas="filtroTarefas()" />
</div>
</template>
