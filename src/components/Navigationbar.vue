<!-- this component represents the left navigation sidebar -->
<!-- the HTML-template for this component: -->
<template>
  <div id="nav">
    <div id="header">
    </div>

    <a
      v-for="quantity in quantities"
      v-bind:key="quantity.name"
      @click="$emit('select-quantity', quantity)"
    >
      <!-- list all quantities as navitems and when you click on a navitem, a custom event named 'select-quantity' is emitted. We listen to the event in the parent element App.vue -->
      {{ quantity.name }}
    </a>

    <div id="footer">Converter by ENPAL</div>
  </div>
</template>

<script>
export default {
  name: "Navigationbar",
  data: function () {
    return {
      quantities: [
        /*  all physical quantities we are working with and their respective units */
        { name: "Family", id: 1, units: ["Person", "kWh"] },
        { name: "You spend", id: 2, units: ["kWh", "Euro"] },
        { name: "You spare", id: 3, units: ["Year", "Euro"] },
      ],
    };
  },
  methods: {
    /*...*/
  },
};
</script>




<style>
main {
  background-size: cover;
}

#nav {
  /* under enpal image part*/
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 20vw;
  background-color: rgba(48, 124, 38, 0.582);
  color: rgb(0, 0, 0);
}
#nav > div {
  display: flex;
  flex-direction: column;
  *justify-content: center;
  align-items: center;
  padding: 20px 0 20px 0;
  text-align: Center;
  vertical-align: middle;
}
#header > img {
  /*enpal image*/
  background-color: rgb(255, 255, 255);
  padding: 20px;
  border-radius: 0%;
  margin: 5px;
}
a {
  /* square of buttons*/
  text-align: center;
  padding: 15px;
  background-color: rgb(44, 143, 96);
  transition: background-color 0.5s;
  cursor: default;
}
a:hover {
  /* the column that holds buttons*/
  background-color: rgb(250, 249, 249);
}
#footer {
  /* last square*/
  height: 10vh;
  background-color: rgb(44, 143, 96);
  margin-top: auto; /* places the footer at the end of the navbar */
  box-sizing: border-box;
  padding: 15px;
  font-size: 0.6em;
}
</style>
