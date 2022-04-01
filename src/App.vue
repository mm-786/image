<template>
  <div id="app">
    <div >
    <div id="printMe" :style="{
        backgroundImage: 'url(' + url + ')',
        height: height,
        width: width,
      }" class="img-box">
      <div>
        <img src="https://www.mundrawala.ml/images/logo.png" :style="{
            position:'relative',
            left: x+'px',
            top: y+'px'
          }" width="75" />
      </div>
    </div>
    </div>
    <div style="padding: 15px">
      <!-- <label>LOGO POSITION </label>
      <select v-model="postion">
        <option v-for="pos in posList" :value="pos" :key="pos">
          {{ pos.replace("-", " ") }}
        </option>
      </select>
      <br /> -->
      Horizontal <input type="range" v-model="x" @change="getX" :max="maxW" /><br />
      Vertical <input type="range" v-model="y" :max="maxH" /><br />
      <button style="padding: 10px; background-color: black; color: azure; font-size: large; width: 100px;"
        @click="print">Print</button>
    </div>
    <div id="previewImg">
    </div>
  </div>
</template>

<script>
  import html2canvas from 'html2canvas';
  import * as htmlToImage from 'html-to-image';
  // import { toPng } from 'html-to-image';
  export default {
    name: "App",
    data() {
      return {
        url: "",
        y:0,
        x: 0,
        height: null,
        width: null,
        maxH: 10,
        maxW: 10,
        postion: "center",
        color: "red",
        posList: [
          "top-left",
          "top-right",
          "top-middle",
          "bottom-left",
          "bottom-middle",
          "bottom-right",
          "left-center",
          "right-center",
          "center",
        ],
      };
    },
    mounted() {
      const img = new Image();
      img.onload = function () {
        console.log(this.width + "x" + this.height);
      };
      img.src ="/assets/bg.jpg";
      this.url = img.src;
      this.height = img.height-2 + "px";
      this.width = img.width-2 + "px";
      this.maxH = img.height-80;
      this.maxW = img.width-80;
      this.$forceUpdate();
    },
    methods: {
      getX(event) {
        console.log(event.target.value);
        this.x = event.target.value
      },
      getY(event) {
        console.log(event.target.value);
        this.y = event.target.value
      },
      print() {
        var node = document.getElementById('printMe');

        htmlToImage.toPng(node)
          .then(function (dataUrl) {
            var img = new Image();
            img.src = dataUrl;
          
            // document.body.appendChild(img);
            var anchorTag = document.createElement("a");
            document.body.appendChild(anchorTag);
            // document.getElementById("previewImg").appendChild(img);
            anchorTag.download = "filename";
            anchorTag.href = img.src;
            anchorTag.target = '_blank';
            anchorTag.click();

          })
          .catch(function (error) {
            console.error('oops, something went wrong!', error);
          });
      },

      async print0() {

        html2canvas(document.getElementById('printMe')).then(function (canvas) {
          console.log(canvas);
          var anchorTag = document.createElement("a");
          document.body.appendChild(anchorTag);
          document.getElementById("previewImg").appendChild(canvas);
          anchorTag.download = "filename.jpg";
          anchorTag.href = canvas.toDataURL();
          anchorTag.target = '_blank';
          anchorTag.click();
        });
        // const options = {
        //   type: "dataURL",
        // };
        // const printCanvas = html2canvas(el, options);
        // const link = document.createElement("a");
        // link.setAttribute("download", "download.png");
        // link.setAttribute(
        //   "href",
        //   printCanvas[0]
        //     .toDataURL("image/png")
        //     .replace("image/png", "image/octet-stream")
        // );
        // link.click();
      },
    },
  };
</script>

<style>
  .img-box {
    border: 5px solid black;
    background-repeat: no-repeat;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    margin-top: 60px;
  }

  .top-left {
    float: left;
    margin: 10px;
  }

  .top-right {
    float: right;
    margin: 10px;
  }

  .top-middle {
    margin: 10px;
    margin-left: 350px;
  }

  .bottom-left {
    margin: 10px;
    margin-top: 420px;
  }

  .bottom-middle {
    margin: 10px;
    margin-top: 420px;
    margin-left: 350px;
  }

  .bottom-right {
    margin: 10px;
    margin-top: 420px;
    float: right;
  }

  .left-center {
    float: left;
    margin: 10px;
    margin-top: 210px;
  }

  .right-center {
    float: right;
    margin: 10px;
    margin-top: 210px;
  }

  .center {
    margin-top: 210px;
    margin-left: 350px;
  }

  .postion {
    position: relative;
    left: 40px;
    top: 40px;
  }
</style>
