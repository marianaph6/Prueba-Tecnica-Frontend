<template>
<v-app >
    <v-container>
    <login />

  </v-container>
</v-app>


</template>

<script>
import login from "@/components/Login";
export default {
  components: {
    login,
  },
  beforeMount(){
    let rol=0;
    let nombres="";
    let id="0";
    localStorage.setItem("user-in", JSON.stringify({ rol, nombres,id }));
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
