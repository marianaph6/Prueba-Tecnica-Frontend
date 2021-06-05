<template>
  <v-card>
    <v-toolbar color="#c6ce00">
      <v-card-title>
        Ficha de perfil
        <v-spacer>
        </v-spacer>
      </v-card-title>
    </v-toolbar>

    <v-card-text>
      <v-container>
        <v-row>
          <v-col cols="12" sm="6" md="4">
            <v-text-field
              label="Cedula Paciente"
              readonly
            >
            </v-text-field>
          </v-col>

          <v-col cols="12" sm="8" md="4">
            <v-text-field
              label="Nombre"
              readonly
            ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6" md="4">
            <v-text-field
              label="Apellidos"
              readonly
            ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6" md="4">
            <v-text-field
              label="Fecha de Nacimiento"
              readonly
            ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6" md="4">
            <v-text-field
              label="Edad"
              readonly
            ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6" md="4">
            <v-text-field
              label="Sexo"
              readonly
            ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6" md="4">
            <v-text-field
              label="Ocupación"
              readonly
            ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6" md="4">
            <v-text-field
              label="Estado Civil"
              readonly
            ></v-text-field>
          </v-col>
        </v-row>
      </v-container>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
    layout:"coordinador"
};
</script>

<style>
</style>