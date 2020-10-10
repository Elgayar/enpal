<template>
  <div id="container">
    <h1>{{ physicalQuantity.name }}</h1>

    <div id="main">
      <input type="text" v-model="fromValue" placeholder="enter amount" />
      <select v-model="fromUnit">
        <!-- choose unit to convert from -->
        <option v-for="unit in physicalQuantity.units" v-bind:key="unit">
          {{ unit }}
        </option>
      </select>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="50"
        height="50"
        viewBox="0 0 24 24"
        fill="rgb(77, 186, 135)"
      >
        <path d="M14 12l-14 9v-18l14 9zm-4-9v4l8.022 5-8.022 5v4l14-9-14-9z" />
      </svg>
      <p id="result">
        {{ result }}
      </p>
      <select v-model="resultUnit" placeholder="result">
        <!-- choose unit to convert to -->
        <option v-for="unit in physicalQuantity.units" v-bind:key="unit">
          {{ unit }}
        </option>
      </select>
    </div>
  </div>
</template>

<!-- the logic of this component -->
<script>
export default {
  name: "Convert",

  props: {
    physicalQuantity: Object,
  },
  data: function () {
    return {
      fromValue: 1,
      fromUnit: "",
      resultUnit: "",
    };
  },
  methods: {
    toElectricity: function (value, unit) {
      // converts a value to its corresponding value in kWh
      switch (unit) {
        case "Euro":
          return value * 0.321;
        case "kWh":
          return value;
        case "Person":
          return value * 28.9;
        default:
          console.log("tokWh conversion failed - unit incorrect");
          return value;
      }
    },
    toEuros: function (value, unit) {
      // converts a  value to its corresponding value in euros
      switch (unit) {
        case "kWh":
          return value / 0.321;
        case "Euro":
          return value;
        case "Year":
          return value * 1750;

        default:
          console.log("toEuro conversion failed - unit incorrect");
          return value;
      }
    },

    toYear: function (value, unit) {
      // converts a value to its corresponding value in years
      switch (unit) {
        case "Euro":
          value = value / 1750;
          return value;

        case "Year":
          return value;
        default:
          console.log("toEuro conversion failed - unit incorrect");
          return value;
      }
    },

    toPerson: function (value, unit) {
      switch (unit) {
        case "kWh":
          value = 0;
          return value;

        case "Person":
          return value;
        default:
          console.log("toEuro conversion failed - unit incorrect");
          return value;
      }
    },
  },
  computed: {
    result: function () {
      let value = parseFloat(this.fromValue); // whatever the user types into the <input> Element, it turns out to be a string. We have to convert it to a number(float)
      if (isFinite(value)) {
        // check if the user really typed in a number (if not, then isFinite returns false)
        switch (
          this.physicalQuantity.name // check in which physicalQuantity we are
        ) {
          case "Family": {
            /* ------------You spend-------------*/
            let valueInElectricity = this.toElectricity(value, this.fromUnit); //
            let valueInPerson = this.toPerson(value, this.fromUnit);
            switch (this.resultUnit) {
              case "Person":
                return parseFloat(valueInPerson.toFixed(5));

              case "kWh":
                return parseFloat(valueInElectricity.toFixed(5));
              default:
                console.log(
                  "Error while converting time - resultUnit not detected!"
                );
                return "...";
            }
          }

          case "You spend": {
            /* ------------You spend-------------*/
            let valueInElectricity = this.toElectricity(value, this.fromUnit);
            let valueInEuro = this.toEuros(value, this.fromUnit);
            switch (this.resultUnit) {
              case "Euro":
                return parseFloat(valueInEuro.toFixed(5));

              case "kWh":
                return parseFloat(valueInElectricity.toFixed(5));
              default:
                console.log(
                  "Error while converting time - resultUnit not detected!"
                );
                return "...";
            }
          }
          /* ----------------------------*/
          case "You spare": {
            /* ------------You spare-------------*/
            let valueInEuro = this.toEuros(value, this.fromUnit);
            let valueInYear = this.toYear(value, this.fromUnit);
            switch (this.resultUnit) {
              case "Euro":
                return parseFloat(valueInEuro.toFixed(5));
              case "Year":
                return parseFloat(valueInYear.toFixed(5));

              default:
                console.log(
                  "Error while converting length - resultUnit not detected!"
                );
                return "...";
            }
          }
        }
      }
      return "..."; // if the user didn't type in a number
    },
  },
};
</script>

<!-- CSS styling for this component. The "scoped" attribute limits the styling exclusibely to THIS component-->
<style>
#container {
  width: 80vw;
  color: rgb(20, 20, 20);
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}
h1 {
  width: 100%;
  background-color: rgba(44, 143, 97, 0.644);
  box-sizing: border-box;
  padding: 30px;
}
#main {
  display: flex;
  box-sizing: border-box;
  padding: 50px;
}

#main > * {
  margin: 0 5px 0 5px;
}
input,
select,
#result {
  padding: 10px;
  border: 2px solid rgb(44, 143, 96);
  border-radius: 10px;
  width: 15vw;
  font-family: "Playfair Display";
  font-size: 1em;
  display: flex;
  align-items: center;
}
</style>
