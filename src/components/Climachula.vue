<template>
  <div class="container">
    <img :src="climeImage" alt="image of weather now" />
    <div class="flex">
      <p>{{ dayOfWeek }},</p>
      <p>{{ dateOfToday }}</p>
      <p>{{ temperature }}º</p>
    </div>
  </div>
</template>

<script>
const WEEKDAY = [
  "Domingo",
  "Lunes",
  "Martes",
  "Miércoles",
  "Jueves",
  "Viernes",
  "Sábado",
];
const MONTHS = [
  "Enero",
  "Febrero",
  "Marzo",
  "Abril",
  "Mayo",
  "Junio",
  "Julio",
  "Agosto",
  "Septiembre",
  "Octubre",
  "Noviembre",
  "Diciembre",
];
import { WeatherController } from "../assets/utils/weather";
const weatherController = new WeatherController();

export default {
  name: "Climachula",
  data() {
    return {
      temperature: 0,
      climeImage: this.climeImage,
    };
  },
  async mounted() {
    await this.renderClimeConditions();
    setInterval(this.renderClimeConditions, 3600000);
  },
  methods: {
    async renderClimeConditions() {
      await weatherController.retrieveData();
      this.temperature = weatherController.temperature;
      this.climeImage = weatherController.getConditionImage();
    },
  },
  computed: {
    dayOfWeek: function () {
      const day = new Date().getDay();
      return WEEKDAY[day];
    },
    dateOfToday: function () {
      const dateOfMonth = new Date().getDate();
      const monthOfYear = new Date().getMonth();
      return `${dateOfMonth} de ${MONTHS[monthOfYear]}`;
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  font-family: "Chulapa";
  font-size: 2.7rem;
  font-weight: 600;
  width: 450px;
  height: 450px;
  margin: 20px auto;
  border: 1px solid rgba(128, 128, 128, 0.797);
  box-shadow: 5px 5px 15px 3px rgba(128, 128, 128, 0.797);
  position: relative;
}

p {
  margin: 2px auto;
  text-align: center;
}
.flex {
  height: 35.9%;
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}
.down {
  margin-top: 40px;
}
</style>
