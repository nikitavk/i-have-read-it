<template>
  <div class = "poem">
    <p>Как будто</p>
    <p>много лет</p>
    <p>спустя</p>
    <p>сорвал</p>
    <p>букет</p>
    <p>поцелуев</p>
    <p>с губ ее</p>
    <p>и поставил</p>
    <p>в вазу цвета зари</p>
    <p>что</p>
    <p>в</p>
    <p>груди.</p>
    <p></p>
    <p>Но</p>
    <p>не зря</p>
    <p>так ждал я.</p>
    <p></p>
    <p>Ведь</p>
    <p>я</p>
    <p>ее</p>
    <p>любил.</p>
  </div>
</template>

<script>
let isLineRead = null;

export default {
  name: "HelloWorld",
  data() {
    return {
      msg: "Welcome to Your Vue.js App"
    };
  },

  mounted() {
    let lines = document.querySelectorAll(".poem p");
    let timers = [];

    isLineRead = new Array(lines.length);
    isLineRead = isLineRead.fill(false, 0, lines.length);

    for (let i = 0; i < lines.length; i++) {
      if (elementInViewport(lines[i])) {
        if (!timers[i] && !isLineRead[i]) {
          timers[i] = read(i);
        }
      } else {
        if (timers[i]) {
          clearTimeout(timers[i]);
          timers[i] = null;
        }
      }
    }

    document.onscroll = e => {
      for (let i = 0; i < lines.length; i++) {
        if (elementInViewport(lines[i])) {
          if (!timers[i] && !isLineRead[i]) {
            timers[i] = read(i);
          }
        } else {
          if (timers[i]) {
            clearTimeout(timers[i]);
            timers[i] = null;
          }
        }
      }

      let allRead = isLineRead.reduce((prev, curr) => {
        return prev && curr;
      }, true);

      if (allRead) {
        document.body.style.background = "lightcoral";
      }
    };
  }
};

const READ_TIME = 500;

function read(i) {
  return setTimeout(() => {
    isLineRead[i] = true;
  }, READ_TIME);
}

function elementInViewport(el) {
  var top = el.offsetTop;
  var left = el.offsetLeft;
  var width = el.offsetWidth;
  var height = el.offsetHeight;

  while (el.offsetParent) {
    el = el.offsetParent;
    top += el.offsetTop;
    left += el.offsetLeft;
  }

  return (
    top < window.pageYOffset + window.innerHeight &&
    left < window.pageXOffset + window.innerWidth &&
    top + height > window.pageYOffset &&
    left + width > window.pageXOffset
  );
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.poem {
  font-size: 20vw;
  display: flex;
  flex-direction: column;
}

.poem p {
  margin: 0;
  padding: 0;
  float: left;
  min-height: 400px;
  text-align: left;
  padding-left: 100px;
}
</style>
