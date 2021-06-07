<template>
  <v-card>
    <v-toolbar color="#c6ce00">
      <v-card-title>
        Ficha de perfil
        <v-spacer> </v-spacer>
      </v-card-title>
    </v-toolbar>

    <v-card-text>
      <v-container>
        <v-form
          class="pa-3 pt-3"
          lazy-validation
          color="#ccf2f4"
          v-model="valid"
          azy-validation
        >
          <v-row>
            <v-col cols="12" sm="6">
              <v-select
              
                required
                :items="tipos_id"
                label="Tipo de identificación"
                v-model="usuario.id_tipo_id"
                item-text="nombre"
                item-value="id_tipo_id"
                filled
                readonly
              ></v-select>
            </v-col>
            <v-col cols="12" sm="6">
              <v-text-field
                name="user"
                label="Nro de Identificación"
                required
                v-model="usuario.id_usuario"
                filled
                readonly
              >
              </v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12" sm="6">
              <v-select
                filled
                readonly
                required
                :items="tipos_rol"
                label="Tipo de rol"
                v-model="usuario.id_rol"
                item-text="nombre"
                item-value="id_rol"
              ></v-select>
            </v-col>
            <v-col cols="12" sm="6">
              <v-text-field
                name="user"
                label="Celular"
                filled
                readonly
                required
                v-model="usuario.celular"
              >
              </v-text-field>
            </v-col>
          </v-row>

          <v-row>
            <v-col cols="12" sm="6">
              <v-text-field
                name="user"
                label="Nombres"
                filled
                readonly
                required
                v-model="usuario.nombres"
              >
              </v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
              <v-text-field
                name="user"
                label="Apellidos"
                filled
                readonly
                required
                v-model="usuario.apellidos"
              >
              </v-text-field>
            </v-col>
          </v-row>
          <v-text-field
            name="user"
            label="Correo"
            filled
            readonly
            required
            v-model="usuario.correo"
          >
          </v-text-field>
        </v-form>
      </v-container>
    </v-card-text>
  </v-card>
</template>

<script>
const url_usuarios = "http://localhost:3001/usuarios/";
const url_tipos_id = "http://localhost:3001/tipos_id/";
const url_tipos_rol = "http://localhost:3001/roles/";
export default {
  beforeMount() {
    this.loadUser();
    this.getTipos_id();
    this.getTipos_Rol();
  },
  data: () => ({
    valid: true,
    usuario: {},
    id_usuario: 0,
    tipos_id: [],
    tipos_rol: [],
  }),
  methods: {
    loadUser() {
      let stringPerfil = localStorage.getItem("user-in");
      let user = JSON.parse(stringPerfil);
      this.id_usuario = user.id;
      console.log("" + this.id_usuario);
      this.getUsuario();
    },
    async getTipos_id() {
      try {
        let response = await this.$axios.get(url_tipos_id);
        this.tipos_id = response.data.content;
      } catch (error) {
        this.tipos_id = [];
        console.error(error);
      }
    },
    async getTipos_Rol() {
      try {
        let response = await this.$axios.get(url_tipos_rol);
        this.tipos_rol = response.data.content;
      } catch (error) {
        this.tipos_rol = [];
        console.error(error);
      }
    },
    async getUsuario() {
      try {
        let { data } = await this.$axios.get(url_usuarios + this.id_usuario);
        this.usuario = data.content;
      } catch (error) {
        this.$swal
          .fire({
            type: "error",
            title: "Oops...",
            text: "El usuario no existe o hubo un error cargandolo.",
            allowEscapeKey: false,
            allowOutsideClick: false,
          })
          .then((result) => {
            if (result.value) {
              if (this.perfil == 2) {
                this.$router.push("coordHome");
              }
              if (this.perfil == 3) {
                this.$router.push("adminHome");
              }
            }
          });
      }
    },
  },
};
</script>
