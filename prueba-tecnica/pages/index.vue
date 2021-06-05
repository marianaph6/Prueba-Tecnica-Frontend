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
    localStorage.setItem("user-in",JSON.stringify(""));
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
