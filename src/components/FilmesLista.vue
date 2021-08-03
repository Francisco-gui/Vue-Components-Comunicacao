<template>
  <div class="row">

    <!-- coluna 1 -->
    <div class="col-8">

      <h2>Filmes</h2>

      <div class="list-group list-group-flush">

        <FilmesListaItem
          v-for="filme in filmes"
          :key="filme.id"
          :filme="filme"
          :class="aplicarClasseAtiva(filme)"
          @selecionarFilme="filmeSelecionado = $event"
        />

      </div>
    </div>

    <!-- coluna 2 -->
    <div class="col-4">

      <FilmesListaItemInfo 
        v-if="!editar"
        :filme="filmeSelecionado"
        @editarFilme="editarFilme"
        />

      <FilmesListaItemEditar 
        v-else
        :filme="filmeSelecionado"
      />
    </div>

  </div>
</template>

<script>
import { eventBus } from '../main';

import FilmesListaItem from './FilmesListaItem.vue'
import FilmesListaItemInfo from './FilmesListaItemInfo.vue'
import FilmesListaItemEditar from './FilmesListaItemEditar.vue'

export default {
  components: {
    FilmesListaItem,
    FilmesListaItemInfo,
    FilmesListaItemEditar
  },
  data() {
    return {
      filmes: [
        { id: 1, titulo: 'Vingadores: Guerra Infinita', ano: 2018 },
        { id: 2, titulo: 'Homem Formiga e a Vespa', ano: 2018 },
        { id: 3, titulo: 'Pantera Negra', ano: 2018 },
        { id: 4, titulo: 'Deadpool 2', ano: 2018 }
      ],
      filmeSelecionado: undefined,
      editar: false
    }
  },
  methods: {
    aplicarClasseAtiva(filmeIterado) {
      return {
        active: this.filmeSelecionado && this.filmeSelecionado.id === filmeIterado.id
      }
    },
    editarFilme(filme) {
      this.editar = true
      this.filmeSelecionado = filme
    },
    atualizarFilme(filmeAtualizado) {
      const indice = this.filmes.findIndex(filme => filme.id === filmeAtualizado.id)
      this.filmes.splice(indice, 1, filmeAtualizado)
      this.filmeSelecionado = undefined
      this.editar = false
    }
  },
  created() {
    eventBus.$on('selecionarFilme', (filmeSelecionado) => {
      this.filmeSelecionado = filmeSelecionado
    }),
    eventBus.$on('atualizarFilme', this.atualizarFilme)
  }
}
</script>