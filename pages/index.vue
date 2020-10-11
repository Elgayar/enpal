<template>
  <div>
    <!-- Notification Popup -->
    <div
      class="px-6 text-cararra bg-green py-4 mb-6 border-0 fixed z-10 bottom-0 left-0 sm:left-3"
      v-bind:class="notificationDisplay">
      
      <span class="inline-block align-middle mr-8">
        {{ notificationText }}
      </span>
      <button
        class="absolute bg-transparent text-2xl font-semibold leading-none right-0 top-0 mt-4 mr-6 outline-none focus:outline-none"
        v-on:click="closeNotification"
      >
        <span>×</span>
      </button>
    </div>

    <div class="">
      <div
        class="relative pt-10 p-2 md:p-10 mt-20 lg:mr-4 rounded-lg bg-green lg:flex-1 shadow-2xl"
      >
        <div class="flex justify-center">
          <h1 class="md:text-5xl text-3xl">
            <b>S</b>olar <b>E</b>npal e<b>X</b>traordinary savings
            calc<b>Y</b>oulator
          </h1>
        </div>
      </div>
    </div>

    <div class="flex flex-row flex-wrap">
      <div
        class="relative pt-10 p-2 md:p-10 mt-20 lg:mr-4 rounded-lg bg-white lg:flex-1 shadow-2xl"
      >
        <div class="bg-green p-5 absolute -top-2 left-3 rounded-lg w-3/6">
          <h1 class="text-xl">Questionnaire</h1>
        </div>

        <div class="flex justify-center p-3">
          <div v-bind:class="startButton">
            <p class="text-lg flex flex-col items-center">
              Welcome to Enpal's Solar Savings Calculator, you'll be asked a few
              questions to know how much you'd save if you use Enpal's green
              services. The more questions you answer, the more accurate results
              you will get, and the more savings we will probably make sure you
              make! When starting this qestionnaire, you will be prompted for
              your location. We would like to assure you that we will never
              share it with anyone nor will we even save it, it will be just
              used during the calculation process to know the weather conditions
              and legal regulations in your city/county
            </p>
            <button
              v-on:click="start"
              class="p-2 text-l border-2 border-white bg-green hover:border-lightBlue rounded-lg m-2 cursor-pointer hover:bg-white hover:text-lightBlue text-white"
            >
              Start
            </button>
          </div>
          <div>
            <h2 class="text-3xl">{{ currentQuestion.question }}</h2>
            <div class="text-center">
              <h3
                v-for="answer in currentQuestion.answers"
                :key="answer.answer"
                v-on:click="pickAnswer(answer)"
                class="p-5 text-xl border-2 border-green rounded-lg m-2 cursor-pointer hover:bg-lightBlue hover:text-white"
              >
                {{ answer.answer }}
              </h3>
            </div>
          </div>
        </div>
        <div class="flex justify-center p-5">
          <button
            v-bind:class="previousButton"
            v-on:click="previousQuestion"
            class="p-2 text-l border-2 border-white bg-lightBlue hover:border-lightBlue rounded-lg m-2 cursor-pointer hover:bg-white hover:text-lightBlue text-white"
          >
            Previous
          </button>
          <button
            v-bind:class="nextButton"
            v-on:click="nextQuestion"
            class="p-2 text-l border-2 border-white bg-lightBlue hover:border-lightBlue rounded-lg m-2 cursor-pointer hover:bg-white hover:text-lightBlue text-white"
          >
            Next
          </button>
        </div>

        <div v-bind:class="finalReport" class="flex flex-col items-center">
          <h1 class="text-2xl">
            You will be saving around {{ savings }} euros yearly.
          </h1>
          <h2 class="text-xl">{{ prize }}</h2>
          <iframe
            src="https://giphy.com/embed/4xpB3eE00FfBm"
            width="450"
            height="430"
            frameBorder="0"
            class="giphy-embed"
            allowFullScreen
          ></iframe>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      startButton: "block",
      nextButton: "hidden",
      previousButton: "hidden",
      notificationDisplay: "hidden",
      notificationText: "",
      questions: [],
      answers: [],
      currentQuestionIndex: 0,
      currentQuestion: {},
      savings: 0,
      finalReport: "hidden",
      prize: "",
    };
  },
  methods: {
    async start() {
      this.questions = [
        {
          index: 0,
          question: "Please enter your location",
        },
        {
          index: 1,
          question: "What shape of roof does your home/building have?",
          answers: [
            { answer: "Flat Roof  ― ", score: 1000 },
            { answer: "Gabel Roof  ⌂", score: 2000 },
            { answer: "Pent Roof  / ", score: 1000 },
          ],
          status: false,
          picked: "",
        },
        {
          index: 2,
          question: "What material is the current roof?",
          answers: [
            { answer: "Asphalt  ", score: 200 },
            { answer: "Tile  ", score: 200 },
            { answer: "Metal ", score: 500 },
            { answer: "Wood", score: 0 },
          ],
          status: false,
          picked: "",
        },
        {
          index: 3,
          question: "How much do you pay for bills in a year?",
          answers: [
            { answer: "400-500 € ", score: 200 },
            { answer: "550-650 €", score: 300 },
            { answer: "700 +€", score: 400 },
          ],
          score: 10000,
          status: false,
          picked: "",
        },
        {
          index: 4,
          question: "How do you describe the weather where you live?",

          answers: [
            { answer: "Mostly rainy", score: 200 },
            { answer: "Mostly sunny", score: 700 },
            { answer: "Mostly grey", score: 300 },
          ],
          score: 10000,
          status: false,
          picked: "",
        },
      ];
      this.startButton = "hidden";
      this.currentQuestion = this.questions[this.currentQuestionIndex];
      this.nextButton = "block";

      var ax = this.$axios;
      var startPos;
      var geoSuccess = async function(position) {
        startPos = position;
        const weather = await ax.$get(
          `http://api.openweathermap.org/data/2.5/weather?lat=${position.coords.latitude}&lon=${position.coords.longitude}&appid=6e22b82df4de52b4c202acd2e0038b4c`
        );
        console.log(weather); // weather data
      };
      navigator.geolocation.getCurrentPosition(geoSuccess);
    },

    nextQuestion() {
      this.currentQuestionIndex++;
      this.currentQuestion = this.questions[this.currentQuestionIndex];
      if (this.currentQuestionIndex + 1 == this.questions.length)
        this.nextButton = "hidden";
      else {
        this.nextButton = "block";
      }
      if (this.currentQuestionIndex == 1) this.previousButton = "block";
    },

    previousQuestion() {
      this.currentQuestionIndex--;
      this.currentQuestion = this.questions[this.currentQuestionIndex];
      if (this.currentQuestionIndex == 0) this.previousButton = "hidden";
      this.nextButton = "block";
    },

    pickAnswer(answer) {
      // also handling if a user changes their answer
      if (!this.questions[this.currentQuestionIndex].status) {
        this.savings += answer.score;
        this.currentQuestion.status = true;
        this.currentQuestion.picked = answer.answer;
      } else {
        for (let i = 0; i < this.currentQuestion.answers.length; i++) {
          if (
            this.currentQuestion.answers[i].answer ==
            this.currentQuestion.picked
          ) {
            this.savings -= this.currentQuestion.answers[i].score;
            this.savings += answer.score;
            this.currentQuestion.picked = answer.answer;
          }
        }
      }
      this.openNotification(
        `Hooray!!! You're now saving up to ${this.savings}!!!`
      );
      if (this.currentQuestionIndex + 1 != this.questions.length)
        this.nextQuestion();
      else {
        this.decidePrize();
        this.finalReport = "block";
        this.previousButton = "hidden";
        this.currentQuestion = {};
      }
    },

    decidePrize() {
      switch (true) {
        case this.savings >= 0 && this.savings <= 2000:
          this.prize = "Now your Christmas gift budget is secured!";
          break;
        case this.savings > 2000 && this.savings < 3000:
          this.prize =
            "In 10 years your child will have the financial support to get a Bachelor's Degree in Germany";
          break;
        case this.savings > 3000:
          this.prize =
            "This can get you a 7-day trip to the Maldives every 2 years!";
          break;
      }
    },

    // Method to show notification popup with a message passed as a parameter.
    openNotification(message) {
      this.notificationText = message;
      this.notificationDisplay = "block";
    },

    // Method to close notification popup.
    closeNotification() {
      this.notificationDisplay = "hidden";
    },
  },
};
</script>
