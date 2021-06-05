<template>
  <v-app>
    <v-container>
      <v-layout row class="text-xs-center">
        <v-flex xs4 sm6>
          <v-container
            style="position: relative; left: 50%"
            class="justify-center"
          >
            <br />
            <center class="white--text">
              <h1>GESTIÓN U&P</h1>
              <h4>Gestión de ususarios y personal</h4>
            </center>
            <br />
            <v-card flat>
              <v-card-title primary-title class="justify-center">
                <h4>Inicio de Sesión</h4>
              </v-card-title>
              <v-form
                v-model="valid"
                class="pa-3 pt-3"
                lazy-validation
                color="#ccf2f4"
                ref="formLogin"
              >
                <v-text-field
                  name="user"
                  label="Nro de Identificación"
                  :rules="rules.required"
                  required
                  v-model="usuario.id"
                  ><v-icon> mdi-account </v-icon>
                </v-text-field>
                <v-text-field
                  name="password"
                  label="Contraseña"
                  type="password"
                  v-model="usuario.contrasenia"
                  :rules="rules.required"
                  required
                ></v-text-field>
                <v-select
                  :items="roles"
                  label="Rol"
                  v-model="usuario.rol"
                  :rules="rules.required"
                >
                </v-select>

                <v-card-actions class="justify-center">
                  <v-btn
                    primary
                    large
                    block
                    class="white--text"
                    color="#06d6a0"
                    @click="login()"
                    >Ingresar</v-btn
                  >
                </v-card-actions>
                <center>
                  <div class="primary--text">
                    <a href="/recuperar-clave">¿Has olvidado tu contraseña?</a>
                  </div>
                  <div>
                    ¿No tienes una cuenta?
                    <span class="primary--text mx-1"
                      ><a href="/Registro">Registrate aquí</a></span
                    >
                  </div>
                </center>
              </v-form>
            </v-card>
          </v-container>
        </v-flex>
      </v-layout>
    </v-container>
  </v-app>
</template>

<script>
const url_roles = "http://localhost:3001/roles/";
const url_usuarios = "http://localhost:3001/usuarios/";
export default {
  beforeMount() {
    this.getRoles();
  },
  data: () => ({
    valid: true,
    roles: [],
    usuario: {
      id: "",
      contrasenia: "",
      rol: "",
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
    async getRoles() {
      try {
        let response = await this.$axios.get(url_roles);
        this.rol = response.data;
        for (var i of this.rol) {
          this.roles.push(i.nombre);
        }
      } catch (error) {
        console.error(error);
      }
    },
    async login() {
      try {
        if (this.$refs.formLogin.validate()) {
          let id_rol = 0;
          let url = "";
          if (this.usuario.rol == "Usuario") {
            id_rol = 1;
            url = "Usuario/userHome";
          } else if (this.usuario.rol == "Coordinador") {
            id_rol = 2;
            url = "Coordinador/coordHome";
          } else if (this.usuario.rol == "Administrador") {
            id_rol = 3;
            url = "Administrador/adminHome";
          }
          let response = await this.$axios.get(url_usuarios);
          let users = response.data;
          let findUser = users.find((x) => {
            return (
              x.id === this.usuario.id &&
              x.contrasenia === this.usuario.contrasenia &&
              x.tipo_rol === id_rol
            );
          });
          if (findUser) {
            delete findUser.password;
              localStorage.setItem("user-in",JSON.stringify(findUser));
              this.$router.push(url);
          } else {
            this.$swal.fire({
              type: "error",
              title: "Oops",
              text: "El id o la contraseña diligenciados son incorrectos.",
              allowEscapeKey: false,
              alowOutsideClick: false,
            });
          }
        } else {
          this.$swal.fire({
            type: "warning",
            title: "Formulario incompleto.",
            text: "Hay campos que deben ser diligenciados.",
          });
        }
      } catch (error) {
        console.log(error);
        this.$swal.fire({
          type: "error",
          title: "Error al iniciar sesión.",
          text: error,
        });
      }
    },
  },
};
</script>
