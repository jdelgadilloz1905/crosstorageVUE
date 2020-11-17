<template>
  <h1>Recibiendo variables</h1>
</template>

<script>
import Vue from "vue";
import integrationDomain from "integration-cross-storage-domain";

Vue.use(integrationDomain);

export default {
  mounted() {
    console.log("paso aqui ");

    integrationDomain.conectOtherDomain();
  }
};
</script>
