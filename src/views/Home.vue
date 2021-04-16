<template>
  <div>
    <span>Authorization</span>
    <input type="text" v-model="authorization" />
    <button @click="getToken">Get Token</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import axios from "axios";
import { stringify } from "qs";

export default defineComponent({
  name: "Home",
  components: {},
  setup() {
    const authorization = ref("");

    const getToken = async () => {
      console.log("Testing Equifax Authentication");

      const options = {
        headers: {
          "Content-Type": "application/x-www-form-urlencoded",
          Authorization: authorization.value,
        },
      };

      const data = stringify({
        grant_type: "client_credentials", // eslint-disable-line
        scope: "https://api.equifax.ca/inquiry/1.0/sts",
      });

      try {
        const authResponse = await axios.post(
          "https://api.uat.equifax.ca/v2/oauth/token",
          data,
          options
        );
        console.dir(authResponse);
      } catch (error) {
        console.dir(error);
      }
    };

    return { authorization, getToken };
  },
});
</script>
