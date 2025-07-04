<script setup>
import { reactive } from 'vue';

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    {
      titulo: 'Estudar ES6',
      concluida: false,
    },
    {
      titulo: 'Estudar SASS',
      concluida: false,
    },
    {
      titulo: 'Ir para academia',
      concluida: true,
    }
  ]
})

const getTarefasPendentes = () => {
 return estado.tarefas.filter(tarefas => !tarefas.concluida)
}

const getTarefasConcluidas = () => {
 return estado.tarefas.filter(tarefas => tarefas.concluida)
}

const getTarefasFiltradas = () => {
  const {filtro} = estado;

  switch (filtro) {
    case 'pendentes' :
      return getTarefasPendentes();
    case 'concluida' :
      return getTarefasConcluidas();
    default:
      return estado.tarefas;
  }
}

const cadastraTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    concluida: false,
  }
  estado.tarefas.push(tarefaNova);
  estado.tarefaTemp = '';
}
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Digite a descrição da tarefa" class="form-control">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="concluidas">Concluídas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefas in getTarefasFiltradas()">
        <input @change="evento => tarefas.concluida = evento.target.checked" :checked="tarefas.concluida" :id="tarefas.titulo" type="checkbox">
        <label :class="{done: tarefas.concluida}" class="ms-3" :for="tarefas.titulo">
          {{tarefas.titulo }}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
  .container {
    background: url(https://images.unsplash.com/photo-1484480974693-6ca0a78fb36b?q=80&w=1472&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D);
    background-size: cover;
    margin: 1rem 2rem;
    opacity: 0.8;  
    text-align: center;
    color: black;
    font-weight: 700;
    display: block;
  }
  
  .done {
    text-decoration: line-through;
    color: rgb(10, 211, 10);
  }
</style>
