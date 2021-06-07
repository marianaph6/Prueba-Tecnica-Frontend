<template>
  <v-card>
    <v-toolbar color="#c6ce00">
      <v-card-title>
        Editar perfil de usuario
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
          ref="formUpdate"
        >
          <v-row>
            <v-col cols="12" sm="6">
              <v-select
                :rules="rules.required"
                required
                :items="tipos_id"
                label="Tipo de identificación"
                v-model="usuario.id_tipo_id"
                item-text="nombre"
                item-value="id_tipo_id"
                
              ></v-select>
            </v-col>
            <v-col cols="12" sm="6">
              <v-text-field
                name="user"
                label="Nro de Identificación"
                :rules="rules.required"
                required
                v-model="usuario.id_usuario"
                filled
                readonly
              >
              </v-text-field>
            </v-col>
          </v-row>
          <v-col cols="12" sm="12">
            <v-select
              :rules="rules.required"
              required
              :items="tipos_rol"
              label="Tipo de rol"
              v-model="usuario.id_rol"
              item-text="nombre"
              item-value="id_rol"
            ></v-select>
          </v-col>
          <v-row>
            <v-col cols="12" sm="6">
              <v-text-field
                name="user"
                label="Nombres"
                :rules="rules.required"
                required
                v-model="usuario.nombres"
              >
              </v-text-field>
            </v-col>
            <v-col cols="12" sm="6">
              <v-text-field
                name="user"
                label="Apellidos"
                :rules="rules.required"
                required
                v-model="usuario.apellidos"
              >
              </v-text-field>
            </v-col>
          </v-row>
          <v-text-field
            name="user"
            label="Correo"
            :rules="emailRules"
            required
            v-model="usuario.correo"
          >
          </v-text-field>
          <v-row>
            <v-col cols="12" sm="6">
              <v-text-field
                name="user"
                label="Celular"
                :rules="rules.required"
                required
                v-model="usuario.celular"
              >
              </v-text-field>
            </v-col>

            <v-col cols="12" sm="6">
              <v-text-field
                name="password"
                label="Contraseña"
                type="password"
                :rules="rules.required"
                required
                v-model="usuario.contrasenia"
              ></v-text-field>
            </v-col>
          </v-row>

          <v-card-actions class="justify-center">
            <v-btn
              primary
              large
              block
              class="white--text"
              color="#06d6a0"
              @click="updateUsuario()"
            >
              Actualizar</v-btn
            >
          </v-card-actions>
        </v-form>
      </v-container>
    </v-card-text>
  </v-card>
</template>

<script>
const url_tipos_id = "http://localhost:3001/tipos_id/";
const url_tipos_rol = "http://localhost:3001/roles/";
const url_usuarios = "http://localhost:3001/usuarios/";
export default {
  beforeMount() {
    this.loadUser();
    this.getTipos_id();
    this.getTipos_Rol();
  },
  data: () => ({
    valid: true,
    perfil: 0,
    usuario: {},
    tipos_id: [],
    tipos_rol: [],
    id_usuario: 0,

    rules: {
      required: [(v) => !!v || "El campo es obligatorio"],
    },
    emailRules: [
      (v) => !!v || "El correo es obligatorio",
      (v) => /.+@.+\..+/.test(v) || "El correo no es valido",
    ],
  }),
  methods: {
    validate() {
      this.$refs.form.validate();
    },
    loadUser() {
      let stringUser = localStorage.getItem("usuario-editar");
      this.id_usuario = JSON.parse(stringUser);
      console.log("" + this.id_usuario);
      this.getUsuario();

      let stringPerfil = localStorage.getItem("user-in");
      let id = JSON.parse(stringPerfil);
      this.perfil = id.rol;
      console.log("" + this.perfil);
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

    async updateUsuario() {
      if (this.$refs.formUpdate.validate()) {
        let user = Object.assign({}, this.usuario);
        let { data } = await this.$axios.put(
          url_usuarios + this.id_usuario,user);
        if (data.ok == true) {
          this.$swal.fire({
            type: "success",
            title: "Operación exitosa.",
            text: "El perfil del usuario se actualizó correctamente.",
          });
          if (this.perfil == 2) {
            this.$router.push("coordHome");
          }
          if (this.perfil == 3) {
            this.$router.push("adminHome");
          }
        } else {
          this.$swal.fire({
            type: "error",
            title: "Error al modificar el perfil del usuario.",
            text: data.message,
          });
        }
      } else {
        this.$swal.fire({
          type: "warning",
          title: "Formulario incompleto.",
          text: "Hay campos que deben ser diligenciados.",
        });
      }
    },
  },
};
</script>
