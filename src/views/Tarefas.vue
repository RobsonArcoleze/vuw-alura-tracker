<script setup lang="ts">
import Formulario from '@/components/Formulario.vue'
import Tarefa from '@/components/Tarefa.vue'
import { TipoNotificacaoEnum } from '@/interface/INotificacao'
import type { ITarefa } from '@/interface/ITarefa'
import { useProjetoStore } from '@/store'
import { ref } from 'vue'

const tarefas = ref<ITarefa[]>([])
const descricao = ref<string>('')
const segundos = ref<number>(0)
const tarefa = ref<ITarefa>()
const store = useProjetoStore()

function adcionarTarefa() {
  if (tarefa.value?.descricao && tarefa.value.segundos > 0) {
    tarefas.value.push({
      descricao: tarefa.value.descricao,
      segundos: tarefa.value.segundos,
      projeto: tarefa.value.projeto,
    })
    store.notificar({
      id: new Date().getTime(),
      texto: `Projeto ${tarefa.value} foi criada!`,
      titulo: 'Sucesso!',
      tipo: TipoNotificacaoEnum.SUCCESS,
      time: 10000,
    })
    descricao.value = ''
    segundos.value = 0
  }
}

function excluirTarefa(tarefa: ITarefa) {
  tarefas.value = tarefas.value.filter((tar) => tar.descricao != tarefa.descricao)
  console.log(tarefa)
}
</script>

<template>
  <Formulario
    @descricao-emitida="descricao = $event"
    @tempo-segundos="segundos = $event"
    @adcionar-tarefa="adcionarTarefa"
    @ao-salvar-tarefa="tarefa = $event"
  />
  <div class="lista" v-for="tar in tarefas" :key="tar.descricao">
    <Tarefa
      v-if="tar.descricao !== '' && tar.segundos > 0"
      :descricao="tar.descricao"
      :segundos="tar.segundos"
      @exclui-tarefa="excluirTarefa"
    />
  </div>
</template>

<style scoped>
.lista {
  padding: 1.25rem;
}
</style>
