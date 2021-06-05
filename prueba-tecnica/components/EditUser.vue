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
          lazy-validation
          color="#ccf2f4"
          v-model="valid"
          azy-validation
          ref="formEditar"
        >
          <v-row>
            <v-col cols="12" sm="6" md="4">
              <v-select
                :rules="rules.required"
                required
                :items="tipos_id"
                label="Tipo de identificación"
                v-model="usuario.tipo_id"
              ></v-select>
            </v-col>

            <v-col cols="12" sm="8" md="4">
              <v-text-field
                name="user"
                label="Nro de Identificación"
                :rules="rules.required"
                required
                v-model="usuario.id"
              >
              </v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                name="user"
                label="Nombres"
                :rules="rules.required"
                required
                v-model="usuario.nombres"
              >
              </v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                name="user"
                label="Apellidos"
                :rules="rules.required"
                required
                v-model="usuario.apellidos"
              >
              </v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                name="user"
                label="Correo"
                :rules="rules.required"
                required
                v-model="usuario.correo"
              >
              </v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                name="user"
                label="Celular"
                :rules="rules.required"
                required
                v-model="usuario.celular"
              >
              </v-text-field>
            </v-col>
          </v-row>
          <v-btn
                class="white--text"
                color="#06d6a0"
                @click="updateUsuario()"
              >
                Actualizar Usuario
              </v-btn>
        </v-form>
      </v-container>
    </v-card-text>
  </v-card>
</template>

<script>
const url_usuarios = "http://localhost:3001/usuarios/";
const url_tipos_id = "http://localhost:3001/tipos_id/";
export default {
  layout: "administrador",
  async asyncData({ params }) {
    let id_user = params.id;
    return { id_user };
  },
  beforeMount() {
    this.loadUser();
    this.getTipos_id();
  },
  data: () => ({
    valid: true,
    usuario: {},
    perfil:null,
    tipos_id: [],
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
    validate() {
      this.$refs.form.validate();
    },
    loadUser() {
      let stringUser = localStorage.getItem("usuario-editar");
      this.usuario = JSON.parse(stringUser);

      let stringPerfil = localStorage.getItem("user-in");
      this.perfil = JSON.parse(stringPerfil);
      console.log(""+ this.perfil.tipo_rol)
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
    async updateUsuario() {
      if (this.$refs.formEditar.validate()) {
        let usuario = Object.assign({}, this.usuario);
        let response = await this.$axios.put(
          url_usuarios + this.usuario.id,
          usuario
        );
        this.$swal.fire({
          type: "success",
          title: "Operación exitosa.",
          text: "La información del usuario se actualizo correctamente.",
        });
        localStorage.setItem("usuario-editar", "");
        if (this.perfil.tipo_rol==2){
            this.$router.push("coordHome");
        }
        if (this.perfil.tipo_rol==3){
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
