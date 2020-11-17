<template>
  <h1>Enviando variables a REACT</h1>
</template>

<script>
import Vue from "vue";
import integrationDomain from "integration-cross-storage-domain";

Vue.use(integrationDomain);

export default {
  mounted() {
    var cdstorage = integrationDomain.crossDomainStorage({
      origin: "http://localhost:8000",
      path: "/"
    });

    cdstorage.setItem(
      "variable1_3000",
      "Esto es una variable creada desde la libreria VUE A REACT..."
    );

    cdstorage.setItem(
      "variable2_3000",
      "Esto es una variable creada desde la libreria VUE A REACT PARTE 2..."
    );
  }
};
</script>
