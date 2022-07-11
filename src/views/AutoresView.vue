<script>
import axios from "axios";
export default {
  data() {
    return {
      autores: [],
      novo_autor: "",
    };
  },
  async created() {
    const autores = await axios.get("http://localhost:4000/autores");
    this.autores = autores.data;
  },
  methods: {
    async salvar() {
      const time = {
        nome: this.novo_autor,
      };
      const autor_criado = await axios.post("http://localhost:4000/autores", time);
      this.autores.push(autor_criado.data);
    },
    async excluir(autor) {
      await axios.delete(`http://localhost:4000/autores/${autor.id}`);
      const indice = this.autores.indexOf(autor);
      this.autores.splice(indice, 1);
    },
  },
};
</script>

<template>
  <div @keydown.enter="salvar" class="form-input">
    <input
      type="text"
      v-model="novo_autor"
      placeholder="Adicionar nome do Autor"
    />
    <button @click="salvar">Salvar</button>
  </div>
  <div class="lista-livros">
    <table style="width: 25%">
      <thead>
        <tr>
          <th style="width: 15%">Nome do Autor</th>
          <th style="width: 10%">Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="autor in autores" :key="autor.id">
          <td>{{ autor.nome }}</td>
          <td>
            <button class="excluir" @click="excluir(autor)">Excluir</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
