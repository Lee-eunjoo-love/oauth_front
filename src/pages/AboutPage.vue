<template>
  <div>
    About...
    <v-btn @click="initUser()">초기화</v-btn>
  </div>
</template>

<script>
import axios from 'axios';
import { onMounted, ref } from 'vue';
import { useCookies } from 'vue3-cookies';
import { decryptData } from '@/plugins/crypto';

export default {
    name: "AboutPage",
    setup() {
      const token = ref('');
      const { cookies } = useCookies();
      const initUser = async () => {
        console.log('initUser ===>', token.value);
        await axios.get("http://localhost:8080/member/valid/user", {
            headers: {
                Authorization: `Bearer ${decryptData(token.value)}`
            }
        })
        .then((response) => console.log(`response: ${response.data}`))
        .catch((error) => console.log('initUser: ERROR : ' + error));
      }

      onMounted(() => {
        token.value = localStorage.getItem("token");//cookies.get("token");
        console.log("onMounted", token.value);
        if (token.value) {
          initUser();
        }
      });

      return {
        initUser,
        cookies,
      };
    },    
}
</script>

<style scoped>

</style>