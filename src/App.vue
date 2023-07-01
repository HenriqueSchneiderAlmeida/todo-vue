<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaTarefa from './components/ListaTarefa.vue';
  
  const estado = reactive ({
    filtros: 'todas',
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
        titulo: 'Estudar GitHub',
        finalizada: true,
      }
  ]
})

const getTarefasPendentes = () => {
return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}                  // (tarefa => tarefa.finalizada === false) assim igual a false acima negação do true -->

const getTarefasFinalizadas = () => {
return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

const getTarefasFiltradas = () => {
  const { filtros } = estado;
  // const filtros = estado.filtros; acima desconstruido.
  switch (filtros) {
    case 'pendentes':
      return getTarefasPendentes();
    case 'finalizadas':
      return getTarefasFinalizadas();
    default: 
      return estado.tarefas;
  } 
}

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = '';
}

// const cadastraTarefa = () => {
//   const tarefaNova = {
//     titulo: estado.tarefaTemp,
//     finalizada: false,
//   }
//   estado.tarefas.push(tarefaNova);
// } // assim pisca a tela e mão mostra legal 
// com o inicio como abaixo resolve. -- (e) e.prev...
// const cadastraTarefa = (e) => {
//   e.preventDefault()
//   const tarefaNova = {
// com o .prevent no submit não precisa disso fica como o 1º e não pisca
</script>

<template>
  <div class="container">
    <Cabecalho  :tarefas-pendentes="getTarefasPendentes().length"></Cabecalho>    
    <Formulario :trocar-filtro="evento => estado.filtros = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastraTarefa="cadastraTarefa"></Formulario>
    <ListaTarefa :tarefas="getTarefasFiltradas()"></ListaTarefa>
  </div>
</template>


<style scoped>
.done  {
  text-decoration: line-through;
}
</style>
