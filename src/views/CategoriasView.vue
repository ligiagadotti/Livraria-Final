<script>
import axios from "axios";
export default {
  data() {
    return {
      categorias: [],
      categoria: {},
    };
  },
  async created() {
    const categorias = await axios.get("http://localhost:4000/categorias");
    this.categorias = categorias.data;
  },
  methods: {
    async salvar() {
      const categoria_criado = await axios.post("http://localhost:4000/categorias", this.categoria);
      this.categorias.push(categoria_criado.data);
    },
    async excluir(categoria) {
      await axios.delete(`http://localhost:4000/categorias/${categoria.id}`);
      const indice = this.categorias.indexOf(categoria);
      this.categorias.splice(indice, 1);
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
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
