<template>
<v-app >
    <v-container>
    <perfil />

  </v-container>
</v-app>


</template>

<script>
import perfil from "@/components/Perfil";
export default {
  layout:"administrador",
  components: {
    perfil,
  },
  beforeMount(){
  },
  data(){
    return{
      user:null
    };
  },
  methods:{
    
  },
};
</script>