<script>
import axios from "axios";
export default {
  data() {
    return {
      editoras: [],
      novo_nomeeditora: "",
      novo_site: "",
    };
  },
    async created() {
    const editoras = await axios.get("http://localhost:4000/editoras");
    this.editoras = editoras.data;
  },
  methods: {
    async salvar() {
      const editora = {
        nome: this.novo_editora,
      };
      const editora_criado = await axios.post("http://localhost:4000/editoras", editora);
      this.editoras.push(editora_criado.data);
    },
    async excluir(editora) {
      await axios.delete(`http://localhost:4000/editoras/${editora.id}`);
      const indice = this.editoras.indexOf(editora);
      this.editoras.splice(indice, 1);
    },
  },
};
</script>

<template>
  <div @keydown.enter="salvar" class="form-input">
    <input
      type="text"
      v-model="novo_nomeeditora"
      placeholder="Nome da Editora"
    />
    <input
      type="text"
      v-model="novo_site"
      placeholder="Link do site da Editora"
    />
    <button @click="salvar">Salvar</button>
  </div>
  <div class="lista-livros">
    <table style="width: 50%">
      <thead>
        <tr>
          <th style="width: 10%">Editora</th>
          <th style="width: 30%">Site</th>
          <th style="width: 10%">Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="editora in editoras" :key="editora.id">
          <td>{{ editora.nomeeditora }}</td>
          <td>{{ editora.site }}</td>
          <td>
            <button class="excluir" @click="excluir(editora)">Excluir</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
