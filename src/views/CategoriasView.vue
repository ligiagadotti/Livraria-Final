<script>
import CategoriasApi from "@/api/categorias.js";
const categoriasApi = new CategoriasApi();
export default {
  data() {
    return {
      categoria: {},
      categorias: [],
    };
  },
  async created() {
    this.categorias = await categoriasApi.buscarTodosOsCategorias();
  },
  methods: {
    async salvar() {
      if (this.categoria.id){
        await categoriasApi.atualizarCategoria(this.categoria);
      } else {
        await categoriasApi.adicionarCategoria(this.categoria);
      }
      this.categorias = await categoriasApi.buscarTodosOsCategorias();
      this.categoria = {};
    },
    async excluir(categoria) {
      await categoriasApi.excluirCategoria(categoria.id);
      this.categorias = await categoriasApi.buscarTodosOsCategorias();
    },
    editar(categoria) {
      Object.assign(this.categoria, categoria);
    },
  },
};
</script>

<template>
  <div class="form-input">
    <input type="text" @keydown.enter="salvar" v-model="categoria.nome" placeholder="Nome da categoria" />
    <!-- <select v-model="novo_tipo">
      <option value="Fantasia">Fantasia</option>
      <option value="Suspense">Suspense</option>
      <option value="Ação">Ação</option>
      <option value="Terror">Terror</option>
      <option value="Romance">Romance</option>
      <option value="Erótico">Erótico</option>
      <option value="True Crimes">True Crimes</option>
      <option value="Biografia">Biografia</option>
      <option value="Religião">Religião</option>
      <option value="Ficção Científica">Ficção Científica</option>
      <option value="Magia">Magia</option>
      <option value="Distopia">Distopia</option>
    </select> -->
    <button @click="salvar">Salvar</button>
  </div>
  <div class="lista-livros">
    <table style="width: 60%">
      <thead>
        <tr>
          <th style="width: 40%">Categoria</th>
          <th style="width: 10%">Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="categoria in categorias" :key="categoria.id">
          <td>{{ categoria.nome }}</td>
          <td>
            <button class="excluir" @click="excluir(categoria)">Excluir</button>
            <button class="editar" @click="editar(categoria)">Editar</button>

          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
