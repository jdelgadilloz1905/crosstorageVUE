<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">crosstorage</h1>
      <div class="links">
        <a
          href="https://nuxtjs.org/"
          target="_blank"
          rel="noopener noreferrer"
          class="button--green"
        >
          Documentation
        </a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          rel="noopener noreferrer"
          class="button--grey"
        >
          GitHub
        </a>
        <hr />
        <div>
          hola desde aqui guardamos el local storage

          <button @click="sendPage">Send</button>

          <a href="https://nuxtjs.org">External Link to another page33</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    sendPage() {
      //console.log("esto es una prueba");
      window.location.href = "http://192.168.1.108:3001/";
    },
  },
  mounted() {
    localStorage.setItem(
      "OTRO_PRIVILEGIO",
      "usted no tiene provilegios desde cross_iframe"
    );

    localStorage.setItem("segunda_variable", "segunda variable");

    //allowed domains (that can use RegExp)

    var whitelist = ["localhost:8000", "localhost", "^.*.domain.com"];

    function verifyOrigin(origin) {
      var domain = origin.replace(/^httpS?:\/\/|:\d{1,4}$/g, "").toLowerCase(),
        i = 0,
        len = whitelist.length;

      while (i < len) {
        if (domain.match(new RegExp(whitelist[i]))) {
          return true;
        }
        i++;
      }

      return false;
    }

    function handleRequest(event) {
      //console.info("eventos", event);
      if (verifyOrigin(event.origin)) {
        var request = JSON.parse(event.data);
        var storage = request.storage;

        if (request.type == "get") {
          value = window[storage].getItem(request.key);
          event.source.postMessage(
            JSON.stringify({
              id: request.id,
              key: request.key,
              value: value,
            }),
            event.origin
          );
        } else if (request.type == "set") {
          window[storage].setItem(request.key, request.value);
        } else if (request.type == "remove") {
          window[storage].removeItem(request.key);
        }
      }
    }

    if (window.addEventListener) {
      //console.info(window.addEventListener);
      window.addEventListener("message", handleRequest, false);
    } else if (window.attachEvent) {
      window.attachEvent("onmessage", handleRequest);
    }
  },
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
