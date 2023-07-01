<script setup>
import { reactive } from 'vue';
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
    <header class="p-5 mb-4 mt-4 bg-light rounded-3"> <!-- bg-light backgrond e rounded-3 para arredondar a borda -->
      <h1>Minhas tarefas</h1>
      <p>Você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
    </header>
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtros = evento.target.value" class="form-control">
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
        <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
        <label :class="{ done: tarefa.finalizada }" :for="tarefa.titulo" class="ms-3">  <!-- (ms margin size) e (me margin end) -->
          <!-- :class="{ done: tarefa.finalizada === true }" pode ser assim tambem -->
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>

  </div>
</template>


<style scoped>
.done  {
  text-decoration: line-through;
}
</style>
