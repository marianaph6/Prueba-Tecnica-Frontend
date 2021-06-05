<template>
  <v-app>
    <v-container>
      <v-layout row class="text-xs-center">
        <v-flex xs4 sm6>
          <v-container
            style="position: relative; left: 50%"
            class="justify-center"
          >
            <v-card flat>
              <v-card-title primary-title class="justify-center">
                <h4>Registro de usuarios</h4>
              </v-card-title>
              <v-form
                class="pa-3 pt-3"
                lazy-validation
                color="#ccf2f4"
                v-model="valid"
                azy-validation
                ref="formRegistro"
              >
                <v-row>
                  <v-col cols="12" sm="6">
                    <v-select
                      :rules="rules.required"
                      required
                      :items="tipos_id"
                      label="Tipo de identificación"
                      v-model="Campos_usuario.tipo_id"
                    ></v-select>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      name="user"
                      label="Nro de Identificación"
                      :rules="rules.required"
                      required
                      v-model="Campos_usuario.id"
                    >
                    </v-text-field>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      name="user"
                      label="Nombres"
                      :rules="rules.required"
                      required
                      v-model="Campos_usuario.nombres"
                    >
                    </v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      name="user"
                      label="Apellidos"
                      :rules="rules.required"
                      required
                      v-model="Campos_usuario.apellidos"
                    >
                    </v-text-field>
                  </v-col>
                </v-row>
                <v-text-field
                  name="user"
                  label="Correo"
                  :rules="rules.required"
                  required
                  v-model="Campos_usuario.correo"
                >
                </v-text-field>
                <v-text-field
                  name="user"
                  label="Celular"
                  :rules="rules.required"
                  required
                  v-model="Campos_usuario.celular"
                >
                </v-text-field>

                <v-row>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      name="password"
                      label="Contraseña"
                      type="password"
                      :rules="rules.required"
                      required
                      v-model="Campos_usuario.contrasenia"
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      name="password"
                      label="Confirmar Contraseña"
                      type="password"
                      :rules="rules.required"
                      required
                      v-model="Password.confirmacion"
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
                    @click="agregarUsuario()"
                  >
                    Registrar</v-btn
                  >
                </v-card-actions>
              </v-form>
            </v-card>
          </v-container>
        </v-flex>
      </v-layout>
    </v-container>
  </v-app>
</template>


<script>
const url_tipos_id = "http://localhost:3001/tipos_id/";
const url_usuarios = "http://localhost:3001/usuarios/";
export default {
  beforeMount() {
    this.getTipos_id();
    this.loadUser();
  },
  data: () => ({
    valid: true,
    tipos_id: [],
    Password: {
      contrasenia: "",
      confirmacion: "",
    },
    perfil: null,
    Campos_usuario: {
      id: "",
      tipo_id: "",
      nombres: "",
      apellidos: "",
      correo: "",
      tipo_rol: 1,
      celular: "",
      contrasenia: "",
    },

    rules: {
      required: [(v) => !!v || "El campo es obligatorio"],
    },
    emailRules: [
      (v) => !!v || "El correo es obligatorio",
      (v) => /.+@.+\..+/.test(v) || "El correo no es valido",
    ],
  }),
  methods: {
    loadUser() {
      let stringPerfil = localStorage.getItem("user-in");
      if (stringPerfil != "") {
        this.perfil = JSON.parse(stringPerfil);
      } else {
        this.perfil = "";
      }
    },
    async getTipos_id() {
      try {
        let response = await this.$axios.get(url_tipos_id);
        this.tipo = response.data;
        for (var i of this.tipo) {
          this.tipos_id.push(i.nombre);
        }
      } catch (error) {
        console.error(error);
      }
    },
    validarPassword() {
      let val = false;
      if (this.Password.contrasenia == this.Password.confirmacion) {
        val = true;
      }
      return val;
    },
    async agregarUsuario() {
      if (this.$refs.formRegistro.validate()) {
        try {
          let usuario = Object.assign({}, this.Campos_usuario);
          let response = await this.$axios.post(url_usuarios, usuario);
          this.$swal.fire({
            type: "success",
            title: "Operación exitosa.",
            text: "El usuario se guardó correctamente.",
          });
        } catch (error) {
          console.log(error);
        }
        this.usuario = "";
        if (this.perfil == "") {
          this.$router.push("/");
        } else {
          this.$router.push("adminHome");
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

<style>
</style>