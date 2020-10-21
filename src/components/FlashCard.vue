<template>
  <div>
    <h1>Create your flashcard!</h1>
    <ul class="flashcard-list">
      <li>
        <div class="flashcard-list">
          <div class="flashcard-form">
            <div
              class="flashcard flashcard-new"
              v-bind:style="{
                backgroundColor: newFrontColor,
              }"
            >
              <div class="card-content-new center">
                <input
                  class="new-card-content-text"
                  placeholder="Front text"
                  type="text"
                  id="front"
                  v-model.trim="newFront"
                  v-bind:style="{
                    color: newFrontColorText,
                    background: newFrontColor,
                    border: newFrontColor,
                    placeholder: newFront,
                    placeholderColor: newFrontColorText
                  }"
                />
              </div>
            </div>
            <div class="card--footer">
              <div class="card--footer--text">Background</div>
              <input
                v-model="newFrontColor"
                type="color"
                class="card--foter--input"
              />
              <div class="card-footer-text">Text</div>
              <input
                v-model="newFrontColorText"
                type="color"
                class="card--foter--input"
              />
            </div>
          </div>
        </div>
      </li>
      <li>
        <div>
          <div class="flashcard flashcard-new" v-bind:style="{
                backgroundColor: newBackColor,
              }">
            <div class="card-content-new center">
                <input
                  class="new-card-content-text"
                  placeholder="Back text"
                  type="text"
                  id="front"
                  v-model.trim="newBack"
                  v-on:keypress.enter="addNew"
                  v-bind:style="{
                    color: newBackColorText,
                    background: newBackColor,
                    border: newBackColor,
                    placeholder: newBack
                    
                  }"
                />
            </div>
          </div>
          <div class="card--footer">
              <div class="card--footer--text">Background</div>
              <input
                v-model="newBackColor"
                type="color"
                class="card--foter--input"
              />
              <div class="card-footer-text">Text</div>
              <input
                v-model="newBackColorText"
                type="color"
                class="card--foter--input"
              />
            </div>
        </div>
      </li>
    </ul>
    <button class="new-card" v-on:click="addNew">Add a New Card</button>
    <span class="error" v-show="error"
      >Flashcards need a front and a back.</span
    >
    <ul class="flashcard-list">
      <li
        v-for="(card, index) in cards"
        v-bind:key="card[index]"
      >
        <div
          @click="card.flipped = !card.flipped"
          v-bind:style="{
            backgroundColor: card.colorFront,
            color: card.colorTextFront,
          }"
          v-show="!card.flipped"
          class="animated flipInX flashcard"
        >
          <span class="delete-card" v-on:click="cards.splice(index, 1)">X</span>
          <div class="card-content center">
            <p v-bind:style="{ fontSize: card.textSizeFront, fontWeight: 'bold' }">
              {{ card.front }}
            </p>
          </div>
        </div>
        <div
          @click="card.flipped = !card.flipped"
          v-bind:style="{
            backgroundColor: card.colorBack,
            color: card.colorTextBack,
          }"
          v-show="card.flipped"
          class="animated flipInX flashcard"
        >
          <span class="delete-card" v-on:click="cards.splice(index, 1)">X</span>
          <div class="card-content center">
            <p v-bind:style="{ fontSize: card.textSizeBack, fontWeight: 'bold' }">
              {{ card.back }}
            </p>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
      isToggle: false,
      show: false,
      cards: [
        {
          front: "What runs around the whole yard without moving?",
          back: "A fence",
          textSizeFront: "16px",
          textSizeBack: "16px",
          colorTextFront: "black",
          colorTextBack: "white",
          colorFront: "white",
          colorBack: "green",
          flipped: false,
        },
        {
          front:
            "How can you leave a room with two legs and return with six legs?",
          back: "Bring a chair back with you.",
          textSizeFront: "16px",
          textSizeBack: "16px",
          colorTextFront: "black",
          colorTextBack: "white",
          colorFront: "white",
          colorBack: "red",
          flipped: false,
        },
      ],
      newFront: "Front Text",
      newBack: "Back Text",
      newFrontColor: "#ffffff",
      newFrontColorText: "#000000",
      newBackColor: "#008000",
      newBackColorText: "#ffffff",
      error: false,
    };
  },
  methods: {
    toggleCard: function(card) {
      card.flipped = !card.flipped;
    },
    addNew: function() {
      //!this.newFront || !this.newBack also works
      if (!this.newFront.length || !this.newBack.length) {
        this.error = true;
      } else {
        this.cards.push({
          front: this.newFront,
          back: this.newBack,
          colorFront: this.newFrontColor,
          colorBack: this.newBackColor,
          colorTextFront: this.newFrontColorText,
          colorTextBack: this.newBackColorText,
          textSizeFront: "16px",
          textSizeBack: "16px",
          flipped: false,
        });
        this.newFront = "Front Text";
        this.newBack = "Back Text";
        this.error = false;
      }
    },
  }
};
</script>
<style lang="scss">
.center {
  text-align: center;
}
.flashcard {
  cursor: pointer;
  border-radius: 10px;
  margin: 20px;
  padding: 25px;
  box-shadow: 0 0px 10px #999999;
  text-align: center;
  overflow-x: hidden;
}

.animated {
  animation-duration: 1s;
  animation-fill-mode: both;
}
@keyframes flipInX {
  from {
    transform: perspective(400px) rotate3d(0, 1, 0, 90deg);
    animation-timing-function: ease-in;
    opacity: 0;
  }
  40% {
    transform: perspective(400px) rotate3d(0, 1, 0, -20deg);
    animation-timing-function: ease-in;
  }
  60% {
    transform: perspective(400px) rotate3d(0, 1, 0, 10deg);
    opacity: 1;
  }
  80% {
    transform: perspective(400px) rotate3d(0, 1, 0, 0deg);
  }
  to {
    transform: perspective(400px);
  }
}
.flipInX {
  backface-visibility: visible !important;
  animation-name: flipInX;
}
.flashcard-form {
  position: relative;
}
button {
  border-radius: 5px;
  border: 1px solid #87cb84;
  background-color: #87cb84;
  padding: 8px 15px;
  outline: none;
  font-size: 14px;
  font-weight: 700;
  color: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
}
label {
  font-weight: 400;
  color: #333;
  margin-right: 10px;
}
// input {
//   border-radius: 5px;
//   border: 2px solid #eaeaea;
//   padding: 10px;
//   outline: none;
// }
ul {
  padding-left: 0;
  display: flex;
  flex-flow: row wrap;
}

element.style {
}
li {
  list-style-type: none;
  padding: 10px 10px;
  transition: all 0.3s ease;
}
body {
  font-family: "Montserrat", sans-serif;
  text-align: center;
}

ul {
  padding-left: 0;
  display: flex;
  flex-flow: row wrap;
}

li {
  list-style-type: none;
  padding: 10px 10px;
  transition: all 0.3s ease;
  min-width: 300px;
  min-height: 150px;
  max-width: 300px;
  max-height: 150px;
}

.container {
  max-width: 100%;
  padding: 2em;
}

.card {
  display: block;
  width: 150px;
  min-width: 150px;
  height: 175px;
  min-height: 175px;
  padding: 80px 50px;
  background-color: #51aae5;
  border-radius: 7px;
  margin: 5px;
  text-align: center;
  line-height: 27px;
  cursor: pointer;
  position: relative;
  color: #fff;
  font-weight: 600;
  font-size: 20px;
  -webkit-box-shadow: 9px 10px 22px -8px #e8e0e8;
  -moz-box-shadow: 9px 10px 22px -8px #e8e0e8;
  box-shadow: 9px 10px 22px -8px #e8e0e8;
  will-change: transform;
}

li:hover {
  transform: scale(1.1);
}

li:nth-child(-n + 3) .card {
  background-color: #e65f51;
}

li:nth-child(2n + 1) .card {
  background-color: #a17de9;
}

li:nth-child(4n) .card {
  background-color: #feca34;
}

li:nth-child(5n-2) .card {
  background-color: #51aae5;
}

li:nth-child(4n + 4) .card {
  background-color: #feca34;
}

li:nth-child(-7n + 7) .card {
  background-color: #e46055;
}

.delete-card {
  position: absolute;
  right: 0;
  top: 0;
  padding: 10px 15px;
  opacity: 0.4;
}

.delete-card:hover,
.error {
  opacity: 1;
  transform: rotate(360deg);
}

.flip-enter-active {
  transition: all 0.4s ease;
}

.flip-leave-active {
  display: none;
}

.flip-enter,
.flip-leave {
  transform: rotateY(180deg);
  opacity: 0;
}

/* Form */
.flashcard-form {
  position: relative;
}

label {
  font-weight: 400;
  color: #333;
  margin-right: 10px;
}

input {
  border-radius: 5px;
  border: 2px solid #eaeaea;
  //   padding: 10px;
  background-color: none;
  outline: none;
  border: none;
  &:focus {
    border: none;
  }
}

button {
  border-radius: 5px;
  border: 1px solid #87cb84;
  background-color: #87cb84;
  padding: 8px 15px;
  outline: none;
  font-size: 14px;
  font-weight: 700;
  color: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
}

button:hover {
  background-color: #70a66f;
}

.error {
  margin-top: 10px;
  display: block;
  color: #e44e42;
  font-weight: 600;
}

.flashcard-list {
  justify-content: center;
}
.flashcard-color {
  position: absolute;
  right: 0;
  top: 0;
  padding: 10px 15px;
  opacity: 0.4;
  transition: all 0.5s ease;
}
.card--footer {
  display: flex;
  justify-content: center;
}
.card--foter--input {
  background-color: white;
  border-color: white;
}
.card-footer-text {
  align-self: center;
  justify-content: center;
}

.card-content-new {
  text-align: center;
}
.card--footer--text {
  align-self: center;
  justify-content: center;  
}
.new-card-content-text {
    font-weight: bold;
    text-align: center;
}
.new-card {
    background: #28a745;
    &:hover{
        background: #218838;
    }
}
</style>
