<script>
export default {
  data() {
    return {
      categorias: [
        {
          titulo: "Harry Potter",
          tipo: "Fantasia",
        },
      ],
      novo_titulo: "",
      novo_tipo: "",
    };
  },
  methods: {
    salvar() {
      if (this.novo_tipo && this.novo_titulo !== "") {
        this.categorias.push({
          titulo: this.novo_titulo,
          tipo: this.novo_tipo,
        });
        this.novo_titulo = "";
        this.novo_tipo = "";
      }
    },
    excluir(categoria) {
      const indice = this.categorias.indexOf(categoria);
      this.categorias.splice(indice, 1);
    },
  },
};
</script>

<template>
  <div @keydown.enter="salvar" class="form-input">
    <input type="text" v-model="novo_titulo" placeholder="Nome do livro" />
    <select v-model="novo_tipo">
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
    </select>
    <button @click="salvar">Salvar</button>
  </div>
  <div class="lista-livros">
    <table style="width: 60%">
      <thead>
        <tr>
          <th style="width: 10%">Título</th>
          <th style="width: 40%">Categoria</th>
          <th style="width: 10%">Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="categoria in categorias" :key="categoria.id">
          <td>{{ categoria.titulo }}</td>
          <td>{{ categoria.tipo }}</td>
          <td>
            <button class="excluir" @click="excluir(categoria)">Excluir</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
