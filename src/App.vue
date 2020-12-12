<template>
  <section class="container" :style="{'--aura-color': auraColor}">
    <div class="box">
      <h1>Aura Checker</h1>
      <p>Welke kleur is het aura hier?</p>
      <button @click="locatorButtonPressed">Check Aura</button>
    </div>
  </section>
</template>

<script>
  export default {
    name: 'App',
    components: {},
    data() {
      return {
        auraColor: "#555"
      }
    },
    methods: {
      locatorButtonPressed() {
        navigator.geolocation.getCurrentPosition(
          position => {
            this.auraColor = this.hslToHex(
              this.clamp(position.coords.longitude, 0, 360),
              this.clamp(position.coords.latitude, 0, 100.0),
              75
            );
            console.log(this.auraColor)
          },
          error => {
            console.log(error.message);
          },
        )
      },
      hslToHex(h, s, l) {
        l /= 100;
        const a = s * Math.min(l, 1 - l) / 100;
        const f = n => {
          const k = (n + h / 30) % 12;
          const color = l - a * Math.max(Math.min(k - 3, 9 - k, 1), -1);
          return Math.round(255 * color).toString(16).padStart(2, '0');
        };
        return `#${f(0)}${f(8)}${f(4)}`;
      },
      clamp(num, min, max) {
        return num <= min ? min : num >= max ? max : num;
      }
    }
  }
</script>

<style lang="scss">
  $background-color: #ffe5c5;
  $dark-color: #2c3e50;

  :root {
    --aura-color: #333;
  }

  html,
  body {
    margin: 0;
    padding: 0;
    min-height: 100vh;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: $dark-color;
    min-height: 100vh;
  }

  .container {
    background-color: var(--aura-color);
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .box {
    width: 60%;
    height: 60%;
    min-width: 250px;
    max-width: 400px;
    margin-top: -60px;

    background-color: $background-color;
    border: 10px solid;
    border-image-source: linear-gradient(60deg, rgba(204, 153, 201, 1) 0%, rgba(158, 193, 207, 1) 20%, rgba(158, 224, 158, 1) 40%, rgba(253, 253, 151, 1) 60%, rgba(254, 177, 68, 1) 80%, rgba(255, 102, 99, 1) 100%);
    border-image-slice: 1;

    padding: 16px;
  }

  button {
    padding: 8px;
    border: solid #2c3e50 3px;
    font-size: 0.8em;
    transition: 0.3s;
    color: $dark-color;
    background: $background-color;
    margin-bottom: 8px;

    &:hover {
      color: $background-color;
      background: $dark-color;
    }
  }

</style>
