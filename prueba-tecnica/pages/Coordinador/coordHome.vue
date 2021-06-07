<template>
  <v-container>
    <v-card>
      <v-card-title>
        Base de datos de Usuarios
        <v-spacer></v-spacer>
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          label="Buscar"
          single-line
          hide-details
        ></v-text-field>
      </v-card-title>
      <v-data-table
        :headers="headers"
        :items="usuarios"
        :search="search"
        class="elevation-1"
      >
        <template v-slot:top>
        
        </template>
        <template v-slot:item.actions="{ item }">
          <v-icon small class="mr-2" @click="editUsuario(item)">
            mdi-pencil
          </v-icon>
        </template>
      </v-data-table>
    </v-card>
  </v-container>
</template>

<script>
const url_usuarios = "http://localhost:3001/usuarios/";
export default {
  layout: "coordinador",
  beforeMount() {
    let token = localStorage.getItem("token");
    this.$axios.setHeader("token", token);
    this.getUsuarios();
  },
  data: () => ({
    search: "",
    usuarios: [],
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: "Tipo id",
        align: "start",
        sortable: false,
        value: "id_tipo_id",
      },
      { text: "Id Usuario", value: "id_usuario" },
      { text: "Nombres", value: "nombres" },
      { text: "Apellidos", value: "apellidos" },
      { text: "Correo", value: "correo" },
      { text: "Celular", value: "celular" },
      { text: "Actions", value: "actions", sortable: false },
    ],
  }),

  computed: {},
  methods: {
    async getUsuarios() {
      try {
        let response = await this.$axios.get(url_usuarios);
        this.usuarios = response.data.content;
      } catch (error) {
        console.error(error);
      }
    },
    editUsuario(item) {
      localStorage.setItem("usuario-editar", JSON.stringify(item.id_usuario));
      this.$router.push('coordEdit');
    },

  },
};
</script>

<style>
</style>