<script setup lang="ts">
import type IProjetos from '@/interface/IProjeto'
import { ref } from 'vue'

const nomeDoProjeto = ref<string>('')
const projetos = ref<IProjetos[]>([])

function salvar() {
  const projeto: IProjetos = {
    id: new Date().toISOString(),
    nome: nomeDoProjeto.value,
  }
  projetos.value.push(projeto)
  nomeDoProjeto.value = ''
}
</script>

<template>
  <section class="projetos">
    <h1 class="title">Projetos</h1>
    <form @submit.prevent="salvar">
      <div class="field">
        <label for="nomeDoProjeto" class="label">Nome do Projeto</label>
        <input
          type="text"
          class="input"
          v-model="nomeDoProjeto"
          id="nomeDoProjeto"
          placeholder="Nome do Projeto"
        />
      </div>
      <div class="field">
        <button class="button" type="submit">Salvar</button>
      </div>
    </form>
    <table class="table is-fullwidth">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="projeto in projetos" :key="projeto.id">
          <td>{{ projeto.id }}</td>
          <td>{{ projeto.nome }}</td>
        </tr>
      </tbody>
    </table>
  </section>
</template>

<style scoped>
.projetos {
  padding: 1.5rem;
}
</style>
