<template>
  <v-container class="mt-4">
    <v-row justify="center">
      <v-col cols="12" class="text-center">
        <h1>Current number: {{ factor }}</h1>
      </v-col>
      <v-col cols="12" class="text-center">
        <v-slider v-model="factor" max="10000" min="2"></v-slider>
      </v-col>
      <v-col cols="12" class="text-center">
        <v-text-field v-model="factor" type="number"></v-text-field>
      </v-col>
      <v-col cols="12" class="text-center">
        <h1>Density: {{ height }}</h1>
      </v-col>
      <v-col cols="12" class="text-center">
        <v-slider v-model="height" max="16" min="4"></v-slider>
      </v-col>
      <v-col cols="12" class="text-center">
        <canvas ref="canvas" width="500" height="500" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Vector from "vector2js";

export default {
  name: "Canvas",
  data: () => ({
    canvas: null,
    factor: 3,
    height: 8,
  }),
  mounted() {
    this.canvas = this.$refs.canvas.getContext("2d");
    this.draw(this.factor);
  },
  watch: {
    factor() {
      this.draw(this.factor);
    },
    height() {
      this.draw(this.factor);
    },
  },
  methods: {
    draw(number) {
      this.canvas.clearRect(0, 0, 500, 500);

      let center = new Vector(250, 250);

      let color = "#0000FF";

      if (this.isPrime(number)) {
        color = "#FF0000";
      }

      for (let i = 9; i < Math.pow(2, this.height); i += 2) {
        let vec = this.getLocation(i, 250);
        let vec2 = this.getLocation(i * number, 250);
        this.drawLine(vec.add(center), vec2.add(center), 1, color);
        this.drawCircle(250, 250, 250, false, color);
      }
    },
    drawLine(from, to, width = 1, color = "#FF0000") {
      this.canvas.strokeStyle = color;
      this.canvas.lineWidth = width;
      this.canvas.beginPath();
      this.canvas.moveTo(from.x, from.y);
      this.canvas.lineTo(to.x, to.y);
      this.canvas.stroke();
    },
    drawCircle(x, y, r, fill = true, color = "#FF0000") {
      this.canvas.beginPath();
      this.canvas.arc(x, y, r, 0, Math.PI * 2);
      this.canvas.closePath();
      this.canvas.fillStyle = color;
      if (fill) {
        this.canvas.fill();
      } else {
        this.canvas.stroke();
      }
    },
    getLocation(number, radius) {
      let row = this.getRow(number);
      let col = this.getRowPos(number, row);
      let rotation = (360 * col) / this.getRowCount(row);
      let vec = new Vector(0, radius);
      vec.rotateDegreesSelf(rotation);
      return vec;
    },
    getRow(number) {
      return Math.floor(Math.log2(number));
    },
    getRowPos(number, row) {
      return Math.floor((number - Math.pow(2, row)) / 2);
    },
    getRowCount(row) {
      return Math.ceil((Math.pow(2, row + 1) - Math.pow(2, row)) / 2);
    },
    isPrime(num) {
      for (let i = 2; i < num; i++) {
        if (num % i === 0) {
          return false;
        }
      }
      return num > 1;
    },
  },
};
</script>

<style scoped>
</style>
