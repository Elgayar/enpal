<template>
  <div class="container">
    <div>
      <!-- Notification Popup -->
      <div
        class="px-6 text-cararra bg-green py-4 mb-6 border-0 fixed z-10 bottom-0 left-0 sm:left-3"
        v-bind:class="notificationDisplay"
      >
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
        <div class="relative bg-green p-2 -top-2 left-3 rounded-lg w-5/6">
          <div class="flex justify-center">
            <h1 class="md:text-5xl text-3xl">
              <b>S</b>olar <b>E</b>npal e<b>X</b>traordinary savings
              calc<b>Y</b>oulator
            </h1>
          </div>
        </div>
      </div>

      <div class="">
        <div
          class="relative pt-10 p-2 md:p-10 mt-20 lg:mr-4 rounded-lg bg-white lg:flex-1 shadow-2xl"
        >
          <div class="bg-green p-5 absolute -top-2 left-3 rounded-lg w-3/6">
            <h1 class="text-xl">Questionnaire</h1>
          </div>

          <div class="flex justify-center p-4">
            <div v-bind:class="startButton">
              <p class="text-lg flex flex-col items-center">
                Welcome to Enpal's Solar Savings Calculator, you'll be asked a
                few questions to know how much you would save if you use Enpal's
                green services. The more questions you answer, the more accurate
                results you will get, and the more savings we will probably make
                sure you make! When starting this questionnaire, you will be
                prompted for your location. We would like to assure you that we
                will never share it with anyone nor will we save it. The
                information will be used during the calculation process to know
                the weather conditions and legal regulations in your
                city/county.
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
          question:
            "Are you ready to save 25.000 - 30.000 € of electricity bills in your lifetime?",
        },
        {
          index: 1,
          question: "What type is your property?",
          answers: [
            { answer: "Residential ", score: 0 },
            { answer: "Commercial", score: 0 },
            { answer: "Non-profit", score: 0 },
          ],
          status: false,
          picked: "",
        },
        {
          index: 2,
          question: "Do you own it or rent it?",
          answers: [
            { answer: "I own it ", score: 50 },
            { answer: "I rent it", score: 25 },
          ],
          status: false,
          picked: "",
        },
        {
          index: 3,
          question:
            "What shape of roof and pitch(in degrees) does your home/building have?",
          answers: [
            { answer: "Flat roof ", score: 0 },
            { answer: "Gable roof  ~30°", score: 0 },
            { answer: "Gable roof ~45°", score: 0 },
            { answer: "Gable roof  ~60°", score: 0 },
            { answer: "Mono-pitched roof ", score: 0 },
          ],
          status: false,
          picked: "",
        },
        {
          index: 4,
          question: "What material is the current roof?",
          answers: [
            { answer: "Asphalt  ", score: 0 },
            { answer: "Tile  ", score: 0 },
            { answer: "Metal ", score: 0 },
            { answer: "Wood", score: 0 },
          ],
          status: false,
          picked: "",
        },
        {
          index: 5,
          question:
            "Do you want to occupy the entire roof area or optimize it for power consumption?",
          answers: [
            { answer: "Only install as many modules as necessary ", score: 40 },
            { answer: "Cover the entire roof area if possible ", score: 70 },
          ],
          status: false,
          picked: "",
        },
        {
          index: 6,
          question: "What is your lifestyle?",
          answers: [
            { answer: "Mostly at home during day ", score: 30 },
            { answer: "Mostly out during day", score: 70 },
          ],
          score: 1000,
          status: false,
          picked: "",
        },
        {
          index: 7,
          question: "How much do you pay for bills in a year?",
          answers: [
            { answer: "500-700 € ", score: 100 },
            { answer: "700-1000 €", score: 140 },
            { answer: "1000 +€", score: 200 },
          ],
          score: 1000,
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
        for (let i = 6; i < this.currentQuestion.answers.length; i++) {
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
        case this.savings >= 0 && this.savings <= 200:
          this.prize = "Now your Christmas gift budget is secured!";
          break;
        case this.savings > 200 && this.savings < 300:
          this.prize =
            "In 10 years you can pay the tuition fee of your child for 6 semesters ";
          break;
        case this.savings > 300:
          this.prize = "Every 2 year you can plan a couple trip";
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

<style scoped>
.container {
  background-image: url("~assets/panel3.jpg");
  background-size: cover;
}
</style>
