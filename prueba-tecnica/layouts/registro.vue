<template>
  <v-app background="../static/images/FondoIndex.png">
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
                <h4>Registro de usuarios</h4>
              </v-card-title>
              <v-form class="pa-3 pt-3" lazy-validation color="#ccf2f4">
                <v-row>
                  <v-col cols="12" sm="6">
                    <v-select
                      :rules="rules.required"
                      required
                      :items="items"
                      label="Tipo de identificación"
                    ></v-select>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      name="user"
                      label="Nro de Identificación"
                      :rules="rules.required"
                      required
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
                    >
                    </v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      name="user"
                      label="Apellidos"
                      :rules="rules.required"
                      required
                    >
                    </v-text-field>
                  </v-col>
                </v-row>
                <v-text-field
                  name="user"
                  label="Correo"
                  :rules="rules.required"
                  required
                >
                </v-text-field>
                <v-text-field
                  name="user"
                  label="Celular"
                  :rules="rules.required"
                  required
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
                    ></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6">
                    <v-text-field
                      name="password"
                      label="Confirmar Contraseña"
                      type="password"
                      :rules="rules.required"
                      required
                    ></v-text-field>
                  </v-col>
                </v-row>

                <v-card-actions class="justify-center">
                  <v-btn primary large block class="white--text" color="#06d6a0"
                    >Registrar</v-btn
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
export default {
  data: () => ({
    items: ["Foo", "Bar", "Fizz", "Buzz"],

    rules: {
      required: [(v) => !!v || "El campo es obligatorio"],
    },
    emailRules: [
      (v) => !!v || "El correo es obligatorio",
      (v) => /.+@.+\..+/.test(v) || "El correo no es valido",
    ],
  }),
};
</script>

<style>
#app {
  background: url("../static/images/FondoIndex.png") no-repeat center fixed !important;
  background-size: cover;
}
</style>