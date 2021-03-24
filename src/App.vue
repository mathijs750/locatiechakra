<template>
  <section class="container" :style="{'--aura-color': auraColor}">
    <div class="box">
      <h1>{{ title }}</h1>
      <p>{{ message }}</p>
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
      auraColor: "#555",
      title: "Aura Checker",
      message: "Welke kleur is het aura hier?",
      possibleMessages: [
        "Als een warm bad.",
        "Ga onmiddelijk ergens anders heen",
        "Kijk uit voor slechte energie",
        "Blijf waakzaam"
      ],
      possibleTitles: [
        "Een Groezelig Aura",
        "Diep Aura",
        "Drukkend Aura",
        "Goede Vibes",
        "Euforisch"
      ],
      }
  },
  methods: {
    locatorButtonPressed()
    {
      navigator.geolocation.getCurrentPosition(
          position => {
            this.auraColor = this.hslToHex(
                // west -180, oost 180
                this.rangeMap(-180, 180, 0, 360, position.coords.longitude),
                // noord +90  , zuid -90
                this.rangeMap(-90, 90, 0, 100, position.coords.latitude),
                70
            )

            // this.message =
            //     'Breetegraad (hoek N/Z): ' + position.coords.latitude +
            //     '\n Lengtegraad (Afstand Gn): ' + position.coords.longitude +
            //     '\n Color: ' + this.auraColor + ' Hue: ' +
            //     this.rangeMap(-180, 180, 0, 360, position.coords.longitude) +
            //     ' Saturation ' +
            //     this.rangeMap(-90, 90, 0, 100, position.coords.latitude)

            this.message = this.possibleMessages[Math.floor((this.possibleMessages.length * Math.random()))]

            this.title = this.possibleTitles[Math.floor((this.possibleTitles.length * Math.random()))]

            console.log(this.auraColor)
          },
          error => {
            console.log(error.message);
          },
      )
    }
  ,
    //degree, percentage, percentage
    hslToHex(hue, saturation, lightness) {
      lightness /= 100;
      const a = Math.min(lightness, 1 - lightness) * saturation / 100;
      const f = n => {
        const k = (n + hue / 30) % 12;
        const color = lightness - a * Math.max(Math.min(k - 3, 9 - k, 1), -1);
        return Math.round(255 * color).toString(16).padStart(2, '0');
      };
      return `#${f(0)}${f(8)}${f(4)}`;
    },
    rangeMap(srcMin, desMin, srcMax, desMax, value)
    {
      //(X-A)/(B-A) * (D-C) + C
      //return (srcValue - srcMin) / (srcMax - srcMin) * (destMax - destMin) + destMin
      //return (value - srcMin) * (desMax - srcMax) / (desMin - srcMin) + srcMax
      return ((Math.sin(value * 2) * desMax) - srcMin) * (desMax - srcMax) / (desMin - srcMin) + srcMax
    }
  
  }
}
</script>

<style lang="scss">
:root {
  --aura-color: #333;
  --dark-color: #3e2c50;
  --background-color: #ffe2d4;
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
  color: var(--dark-color);
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
  width: 40ch;
  padding: 1.5rem;
  margin-top: -4rem;
  border: 1rem solid;
  background-color: var(--background-color);
  border-image-source: linear-gradient(60deg, rgba(204, 153, 201, 1) 0%, rgba(158, 193, 207, 1) 20%, rgba(158, 224, 158, 1) 40%, rgba(253, 253, 151, 1) 60%, rgba(254, 177, 68, 1) 80%, rgba(255, 102, 99, 1) 100%);
  border-image-slice: 1;

  h1, p {
    transition: 0.3s;
  }

  h1 {
    margin-top: 0;
  }
}

button {
  padding: .75rem 1.25rem;

  border: solid var(--dark-color) 3px;
  font-size: 1em;
  transition: 0.3s;
  color: var(--dark-color);
  background: var(--background-color);

  &:hover {
    color: var(--background-color);
    background: var(--dark-color);
  }
}

</style>
