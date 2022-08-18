<template>
  <div id="VG_LoadingPageForm">
    <div id="LoadingBox" class="LoadingBox">
      <div class="SubLoadingBox">
        <div class="LoadingImg"><img src="./assets/iconVersil.png" /></div>
        <div id="LoadingHeight" class="LoadingHover"><img src="./assets/iconVersil.png" alt="" /></div>
        <div class="TextBox"><span id="NumberText">0</span><span>%</span>
          <div class="SubTextBox">
            <span>{{ ProcessText }}</span>
          </div>
        </div>
      </div>
      <div class="ServerName">
        <span class="ServerNameSpans" v-for="ServerName in ServerName[Math.floor(Math.random() * ServerName.length)]"
          :key="ServerName[0]">{{
              ServerName
          }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',

  data() {
    return {
      loaded: 15,
      Boxheight: 105,
      ProcessText: 'Loading Views',
      ServerName: [['V', 'E', 'R', 'S', 'I', 'L', ' ', 'G', 'A', 'M', 'E'],
      ['V', 'e', 'R', 's', 'i', 'L', ' ', 'G', 'a', 'M', 'e'],
      ['V', 'e', 'r', 's', 'i', 'L', ' ', 'G', 'a', 'm', 'E'],
      ['V', 'e', 'R', 'S', 'i', 'L', ' ', 'G', 'a', 'm', 'e'],
      ],
    }
  },
  created() {
    const SetNumber = setInterval(() => {
      if (document.getElementById("NumberText").innerHTML == 100) {
        document.getElementById("LoadingBox").style.opacity = "0";
        setTimeout(() => {
          document.getElementById("VG_LoadingPageForm").innerHTML = "";
        }, 700);
        this.DestroyWebView();
        return clearInterval(SetNumber);
      }
      if (document.getElementById("NumberText").innerHTML >= this.loaded) {
        return;
      }
      document.getElementById("NumberText").innerHTML =
        parseInt(document.getElementById("NumberText").innerHTML) + 1;

      this.Boxheight--;
      document.getElementById("LoadingHeight").style.height = this.Boxheight + "px";
    }, 10);
    setTimeout(() => {
      this.SetLoader(3);
    }, 1500);
    setTimeout(() => {
      this.SetLoader(5);
    }, 3000);
    setTimeout(() => {
      this.SetLoader(7);
    }, 4500);
    setTimeout(() => {
      this.SetLoader(8);
    }, 6000);
    setTimeout(() => {
      this.SetLoader(9);
    }, 8000);

    if ("alt" in window) {
      alt.on("ClientWEB:LoadingPageWebView:FullLoaded", () => {
        this.SetNum(100);
      });
      alt.on("ClientWEB:LoadingPageWebView:ProcessText", (Text) => {
        this.ProcessText = Text;
      });
    }
  },
  methods: {
    SetNum(num) {
      this.loaded = num;
    },
    SetLoader(num) {
      if (this.loaded != 100) {
        let number = `${num}${Math.floor(Math.random() * 10)}`;
        this.loaded = parseInt(number);
      }
    },
    isWebViewReady(Status) {
      if ("alt" in window && Status) {
        alt.emit("ClientWEB:LoadingPageWebView:Ready");
      }
    },
    DestroyWebView() {
      if ("alt" in window) {
        setTimeout(() => {
          alt.emit("ClientWEB:LoadingPageWebView:Destroy");
        }, 300);
      }
    }
  },
  mounted() {
    const ServerNameSpans = document.getElementsByClassName('ServerNameSpans')
    for (let i = 0; i < 3; i++) {
      ServerNameSpans[Math.floor(Math.random() * ServerNameSpans.length)].classList.add('ServerNameAnimationUpper')
      ServerNameSpans[Math.floor(Math.random() * ServerNameSpans.length)].classList.add('ServerNameAnimationLower')
    }
    ServerNameSpans[Math.floor(Math.random() * ServerNameSpans.length)].classList.add('ServerNameOffCharacter')
    this.isWebViewReady(true)

  }
}
</script>

<style>
@font-face {
  font-family: "monoton";
  src: url("./assets/fonts/monoton.ttf");
}

.LoadingBox {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: #1b1b1b;
  transition: 0.5s;
  z-index: 1000000;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.SubLoadingBox {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 100%;
  height: 160px;
  overflow: hidden;
  margin: auto;
  text-align: center;
}

.LoadingImg,
.LoadingHover {
  position: absolute;
  top: 0;
  left: 0;
  text-align: center;
  width: 100%;
  height: 100px;
  overflow: hidden;
}

.LoadingImg img,
.LoadingHover img {
  height: 100px;
}

.LoadingHover img {
  filter: brightness(0.5);
}

.TextBox {
  color: #cccccc;
  font-family: sans-serif;
  margin-top: 110px;
}

.SubTextBox {
  margin-top: 10px;
}

.ServerName {
  position: absolute;
  font-family: monoton;
  bottom: 10px;
  left: 10px;
  color: #cccccc;
  font-size: 65px;
  word-spacing: 10px;
  letter-spacing: 5px;
  text-shadow: 0 0 5px #ccc;
}

.ServerNameAnimationUpper {
  animation: upper 6s linear infinite;
}

.ServerNameAnimationLower {
  animation: upper 9s linear infinite;
}

.ServerNameOffCharacter {
  opacity: 0.4;
  text-shadow: none;
}

@keyframes upper {

  0%,
  19.999%,
  22%,
  62.999%,
  64%,
  64.999%,
  70%,
  100% {
    opacity: .99;
    text-shadow: 0 0 5px #ccc;
  }

  20%,
  21.999%,
  63%,
  63.999%,
  65%,
  69.999% {
    opacity: .4;
    text-shadow: none;
  }
}

@keyframes lower {

  0%,
  12%,
  18.999%,
  23%,
  31.999%,
  37%,
  44.999%,
  46%,
  49.999%,
  51%,
  58.999%,
  61%,
  68.999%,
  71%,
  85.999%,
  96%,
  100% {
    opacity: .99;
    text-shadow: 0 0 5px #ccc;
  }

  19%,
  22.99%,
  32%,
  36.999%,
  45%,
  45.999%,
  50%,
  50.99%,
  59%,
  60.999%,
  69%,
  70.999%,
  86%,
  95.999% {
    opacity: .4;
    text-shadow: none;
  }
}

@media only screen and (max-width: 1500px) {
  .ServerName {
    font-size: 50px;
  }
}
@media only screen and (max-width: 1000px) {
  .ServerName {
    font-size: 40px;
  }
}
</style>
