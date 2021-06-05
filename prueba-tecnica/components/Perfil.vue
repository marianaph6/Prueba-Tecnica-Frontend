<template>
  <v-card>
    <v-toolbar color="#c6ce00">
      <v-card-title>
        Ficha de Perfil
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
        >
          <v-row>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                :rules="rules.required"
                required
                :items="tipos_id"
                label="Tipo de identificación"
                v-model="usuario.tipo_id"
                readonly
              ></v-text-field>
            </v-col>

            <v-col cols="12" sm="8" md="4">
              <v-text-field
              readonly
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
                readonly
              >
              </v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
              readonly
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
              readonly
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
              readonly
                name="user"
                label="Celular"
                :rules="rules.required"
                required
                v-model="usuario.celular"
              >
              </v-text-field>
            </v-col>
          </v-row>
        </v-form>
      </v-container>
    </v-card-text>
  </v-card>
</template>

<script>

export default {
  beforeMount() {
    this.loadUser();
    //this.getTipos_id();
  },
  data: () => ({
    valid: true,
    usuario: {},
    tipos_id: [],

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
      let stringUser = localStorage.getItem("user-in");
      this.usuario = JSON.parse(stringUser);
    },

  },
};
</script>
