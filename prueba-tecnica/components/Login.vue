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
                  v-model="usuario.id_usuario"
                  type="number"
                  class="inputPrice"
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
const url_roles = "http://localhost:3002/roles/";
const url_usuarios = "http://localhost:3001/login";
export default {
  beforeMount() {
    //this.getRoles();
  },
  data: () => ({
    valid: true,
    roles: [],
    usuario: {},
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
          let url = "";
          let response = await this.$axios.post(url_usuarios,this.usuario);
          console.log(response.data);
          let info = response.data;
          
          if (info.ok == true) {
            let rol = info.content.rol;
            let nombres = info.content.nombres;
            let id= info.content.id;
            let token = info.content.token;
            localStorage.setItem("token", token);
            localStorage.setItem("user-in", JSON.stringify({ rol, nombres,id }));
            if (rol == 1) {
              this.$router.push("Usuario/userHome");
            } else if (rol == 2) {
              this.$router.push("Coordinador/coordHome");
            } else if (rol == 3) {
              this.$router.push("Administrador/adminHome");
            }
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
