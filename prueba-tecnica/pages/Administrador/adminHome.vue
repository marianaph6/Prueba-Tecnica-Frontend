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
          <v-toolbar flat>
            <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on" @click="agregarUsuario">
              Nuevo usuario
            </v-btn>
            <v-divider class="mx-4" inset vertical></v-divider>
            <v-spacer></v-spacer>
          </v-toolbar>
        </template>
        <template v-slot:item.actions="{ item }">
          <v-icon small class="mr-2" @click="editUsuario(item)">
            mdi-pencil
          </v-icon>
          <v-icon small @click="deleteUsuario(item)" > mdi-delete </v-icon>
        </template>
        <template v-slot:no-data>
          <v-btn color="primary"> Reset </v-btn>
        </template>
      </v-data-table>
    </v-card>
  </v-container>
</template>

<script>
const url_usuarios = "http://localhost:3001/usuarios/";
export default {
  layout: "administrador",
  beforeMount() {
    this.getUsuarios();
  },
  data: () => ({
    search: "",
    usuarios: [],
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: "Id",
        align: "start",
        sortable: false,
        value: "id",
      },
      { text: "Tipo id", value: "tipo_id" },
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
        this.usuarios = response.data;
      } catch (error) {
        console.error(error);
      }
    },
    deleteUsuario(item) {
      this.$swal
        .fire({
          type: "warning",
          title: "¿Está seguro de eliminar el usuario?",
          text: "Al borrar el usuario este se prodrá recuperar.",
          allowEscapeKey: false,
          allowOutsideClick: false,
          showCancelButton: true,
        })
        .then(async (result) => {
          if (result.value) {
            try {
              let url = url_usuarios + item.id;
              await this.$axios.delete(url);
              this.$swal.fire({
                type: "success",
                title: "Operación exitosa.",
                text: "El usuario se eliminó correctamente.",
              });
              this.getUsuarios();
            } catch (error) {
              this.$swal.fire({
                type: "error",
                title: "Ha ocurrido un problema al eliminar el usuario",
                text: error.toString(),
              });
            }
          }
        });
    },
    editUsuario(item) {
      localStorage.setItem("usuario-editar", JSON.stringify(item));
      this.$router.push('adminEdit');
    },
        agregarUsuario() {
      
      this.$router.push('adminNewUser');
    },

  },
};
</script>

<style>
</style>