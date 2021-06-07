<template>
  <v-app>
    <v-navigation-drawer v-model="openMenu" fixed app>
      <v-list>
        <v-list-item
          v-for="item in items"
          :key="item.id"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>

          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
      <template v-slot:append>
        <div>
          <v-btn block @click="cerrarSesion()">
            <v-icon> mdi-exit-to-app </v-icon>
            Cerrar sesion
            
          </v-btn>
        </div>
      </template>
    </v-navigation-drawer>

    <v-app-bar fixed app color=#26bb8f>
      <v-app-bar-nav-icon @click.stop="openMenu = !openMenu" />
      <v-toolbar-title class="flex text-center">Administrador</v-toolbar-title>

      <v-dialog v-model="dialog" persistent max-width="350">
        <template v-slot:activator="{ on, attrs }">
          <v-btn icon dark v-bind="attrs" v-on="on">
            <v-icon>mdi-help</v-icon>
          </v-btn>
        </template>
        <v-card>
          <v-card-title class="text-h5">
            Que puedes hacer con tu rol de administrador?
          </v-card-title>
          <v-card-text
            >Como administrador vas a tener acceso a la gestión completa de todos los usuarios
            en el sistema, lo cual incluye su busqueda, creación, actualización y eliminación.
            Tambien vas a poder tener una vista de la ficha de tu perfil</v-card-text
          >
          <v-card-actions>
            <v-spacer></v-spacer>

            <v-btn color="green darken-1" text @click="dialog = false">
              Cerrar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-app-bar>

    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>

    <v-footer app>
      <v-spacer></v-spacer>
      <div>
        <center>
          <span>&copy; {{ year }}</span>
          <br />
        </center>
      </div>
      <v-spacer></v-spacer>
    </v-footer>
  </v-app>
</template>

<script>
import { auth } from "@/mixins/auth";
export default {
  mixins: [auth],
  beforeMount() {
    let token = localStorage.getItem("token");
    this.$axios.setHeader("token", token);
    this.loadUser();
    this.verifyToken();
  },
  data() {
    return {
      user: null,
      dialog: false,
      year: new Date().getFullYear(),
      openMenu: false,
      items: [
        {
          id: "home",
          icon: "mdi-home",
          title: "Inicio",
          to: "/Administrador/adminHome",
        },
        {
          id: "productos",
          icon: "mdi-account",
          title: "Perfil",
          to: "/Administrador/adminPerfil",
        },
      ],
    };
  },
  methods: {
    loadUser() {
      let stringUser = localStorage.getItem("user-in");
      this.user = JSON.parse(stringUser);
      this.validRol(this.user);
    },
    validRol(user) {
      if (!user || user.rol != 3) {
        this.$router.push("/");
      }
    },
    cerrarSesion() {
      localStorage.clear();
      this.$router.push("/");
    },
  },
};
</script>

<style scoped>
#app {
  background: url("../static/images/FondoPages.png") no-repeat center fixed !important;
  background-size: 1366px 788px;
}
</style>