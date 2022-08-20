<template>
  <div class="header">
    <div class="header__conteiner">
      <div class="header__title">Конвертер валют</div>
      <div class="header__course">
        <div class="USD" v-for="(data, index) in data.slice(0, 1)" :key="index">
          <div class="header__course_title">USD</div>
          <div class="buy">{{ Math.round(data.buy * 100) / 100 }}</div>
          <div class="sale">{{ Math.round(data.sale * 100) / 100 }}</div>
        </div>
        <div class="EUR" v-for="(data, index) in data.slice(1, 2)" :key="index">
          <div class="header__course_title">EUR</div>
          <div class="buy">{{ Math.round(data.buy * 100) / 100 }}</div>
          <div class="sale">{{ Math.round(data.sale * 100) / 100 }}</div>
        </div>
      </div>
    </div>
  </div>
  <div class="body">
    <div class="body__container">
      <div class="body__title">Конвертер №1</div>
      <div class="converter__one">
        <div class="converter__one_left">
          <label class="label" for="UAH">Гривні</label>
          <label class="label" for="USD">Долари</label>
          <label class="label" for="EUR">Євро</label>
        </div>
        <div class="converter__one_right">
          <input
            class="input"
            id="UAH"
            type="number"
            v-on:input="convUAH"
            v-model="UAH"
            placeholder="Введіть сумму в гривнях"
          />
          <input
            class="input"
            id="USD"
            type="number"
            v-on:input="convUSD"
            v-model="USD"
            placeholder="Введіть сумму в доларах"
          />
          <input
            class="input"
            id="EUR"
            type="number"
            v-on:input="convEUR"
            v-model="EUR"
            placeholder="Введіть сумму в євро"
          />
        </div>
      </div>
      <div class="body__title">Конвертер №2</div>
      <div class="converter__two">
        <section>
          <label for="UAH_2">Гривні</label>
          <input
            type="number"
            id="UAH_2"
            v-on:input="conv_1"
            v-model="convOne.UAH"
            placeholder="Введіть сумму в гривнях"
          />
          <div class="first">В доларах: {{ convOne.USD }}</div>
          <div class="second">В євро: {{ convOne.EUR }}</div>
        </section>
        <section>
          <label for="USD_2">Долари</label>
          <input
            type="number"
            id="USD_2"
            v-on:input="conv_2"
            v-model="convTwo.USD"
            placeholder="Введіть сумму в доларах"
          />
          <div class="first">В гривнях: {{ convTwo.UAH }}</div>
          <div class="second">В євро: {{ convTwo.EUR }}</div>
        </section>
        <section>
          <label for="EUR_2">Євро</label>
          <input
            type="number"
            id="EUR_2"
            v-on:input="conv_3"
            v-model="convThree.EUR"
            placeholder="Введіть сумму в євро"
          />
          <div class="first">В гривнях: {{ convThree.UAH }}</div>
          <div class="second">В доларах: {{ convThree.USD }}</div>
        </section>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      data: [],
      UAH: '',
      USD: '',
      EUR: '',
      convOne: [{ UAH: 0, USD: 0, EUR: 0 }],
      convTwo: [{ UAH: 0, USD: 0, EUR: 0 }],
      convThree: [{ UAH: 0, USD: 0, EUR: 0 }],
    };
  },
  mounted() {
    axios
      .get("https://api.privatbank.ua/p24api/pubinfo?exchange&json&coursid=11")
      .then((response) => (this.data = response.data));
  },
  methods: {
    convUAH() {
      this.USD = Math.round((this.UAH / this.data[0].buy)*100)/100;
      this.EUR = Math.round((this.UAH / this.data[1].buy)*100)/100;
    },
    convUSD() {
      this.UAH = Math.round((this.USD * this.data[0].buy)*100)/100;
      this.EUR =
        Math.round((this.USD * ((this.data[0].buy * 100) / this.data[1].buy)) / 100*100)/100;
    },
    convEUR() {
      this.UAH = Math.round(this.EUR * this.data[1].buy*100)/100;
      this.USD =
        Math.round((this.EUR * ((this.data[1].buy * 100) / this.data[0].buy)) / 100*100)/100;
    },
    conv_1() {
      this.convOne.USD = Math.round(this.convOne.UAH / this.data[0].buy*100)/100;
      this.convOne.EUR = Math.round(this.convOne.UAH / this.data[1].buy*100)/100;
    },
    conv_2() {
      this.convTwo.UAH = Math.round(this.convTwo.USD * this.data[0].buy*100)/100;
      this.convTwo.EUR =
        Math.round((this.convTwo.USD * ((this.data[0].buy * 100) / this.data[1].buy)) /
        100*100)/100;
    },
    conv_3() {
      this.convThree.UAH = Math.round(this.convThree.EUR * this.data[1].buy*100)/100;
      this.convThree.USD =
        Math.round((this.convThree.EUR * ((this.data[1].buy * 100) / this.data[0].buy)) /
        100*100)/100;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.header {
  width: 100%;
  height: 8vh;
  background-color: #fff;
  border-bottom: 1px solid teal;
}
.header__conteiner {
  max-width: 1280px;
  height: 100%;
  margin: auto;
  background-color: #f8f8f8;
}
.header__title {
  display: inline;
  padding-left: 20px;
  font-family: "Proxima Nova", sans-serif;
  font-style: normal;
  font-weight: 700;
  font-size: 32px;
  line-height: 230%;
}
.header__course {
  display: flex;
  float: right;
  padding-right: 20px;
}
.header__course_title {
  font-family: sans-serif;
  font-weight: bold;
  margin-top: 5px;
  margin-bottom: 5px;
}
.USD {
  float: left;
  width: 100px;
  display: inline;
  font-family: sans-serif;
}
.EUR {
  display: inline;
  width: 100px;
  font-family: sans-serif;
}
.body__container {
  max-width: 1280px;
  height: 92vh;
  margin: auto;
  padding: 15px;
  background-color: #f9f9f9;
}
.body__title {
  padding-left: 20px;
  font-family: "Proxima Nova", sans-serif;
  font-style: normal;
  font-weight: 500;
  font-size: 26px;
  line-height: 230%;
}
.converter {
  margin: 15px;
  text-align: center;
}
.converter__one {
  display: flex;
}
.converter__one_left {
  width: 40%;
  display: block;
}
.converter__one_right {
  width: 60%;
  display: inline;
}
.converter__one_left label {
  margin: 15px;
  width: 100%;
  height: 30px;
  padding-top: 5px;
  padding-right: 25px;
  text-align: end;
  display: block;
  font-family: "Proxima Nova", sans-serif;
  font-style: normal;
  font-weight: 400;
  font-size: 18px;
}
.converter__one_right input {
  margin: 15px;
  width: 300px;
  height: 30px;
  padding-left: 10px;
  border-radius: 8px;
  border: 1px solid teal;
  font-family: "Proxima Nova", sans-serif;
  font-style: normal;
  font-weight: 400;
  font-size: 24px;
  display: block;
}
.converter__two section {
  display: flex;
  justify-content: center;
  margin: 10px;
}
.converter__two section label {
  padding: 7px;
  width: 70px;
  text-align: center;
  height: 30px;
  font-family: "Proxima Nova", sans-serif;
  font-style: normal;
  font-weight: 400;
  font-size: 18px;
}
.converter__two section input {
  margin-left: 15px;
  width: 250px;
  height: 30px;
  padding-left: 10px;
  border-radius: 8px;
  border: 1px solid teal;
  font-family: "Proxima Nova", sans-serif;
  font-style: normal;
  font-weight: 400;
  font-size: 24px;
  display: block;
}
.converter__two section .first {
  min-width: 300px;
  margin-inline: 25px;
  overflow: hidden;
  height: 30px;
  font-family: "Proxima Nova", sans-serif;
  font-style: normal;
  font-weight: 400;
  font-size: 18px;
}
.converter__two section .second {
  min-width: 300px;
  margin-inline: 5px;
  overflow: hidden;
  height: 30px;
  font-family: "Proxima Nova", sans-serif;
  font-style: normal;
  font-weight: 400;
  font-size: 18px;
  display: inline;
}
input:focus {
  outline: none !important;
  border: 2px solid teal;
  box-shadow: 0 0 7px #719ece;
}
input::placeholder {
  font-size: 18px;
}
</style>
