<template>
  <div >
   <div class="home">
      <div class="MainUi">
      <!-- <h1>Welcome To Push Up Coach</h1> -->
         <h1> Push Ups</h1> 
         <h2>{{counter}}</h2>  
    <b-button v-if="!setStarted" variant=outline-light @click="init()">Start Set</b-button>
    <b-button v-else @click="stop()">End Set</b-button>
    </div>
    <video id="video" width="300" height="300" playsinline autoplay></video>
   </div>
    <!-- <div id="label-container"></div> -->
    <!-- {{currentPosistion}}
    UP:{{upProb}} | DOWN:{{downProb}} -->
  </div>
</template>

<script>
  export default {
    name: 'Home',
    data() {
      return {
        URL:"https://teachablemachine.withgoogle.com/models/DoymK5tqx/",
        model:{},
        webcam:{},
        video:{},
        labelContainer:{},
        maxPredictions:{},
        downProb:0,
        upProb:0,
        currentPosistion:'',
        counter:0,
        setStarted:false,
      }
    },
    async mounted(){
    },
    methods: {
      async init() {
        this.setStarted=true
        this.video = document.getElementById('video');
        const modelURL = this.URL + "model.json";
        const metadataURL = this.URL + "metadata.json";
        this.model = await tmImage.load(modelURL, metadataURL);
        this.maxPredictions = this.model.getTotalClasses();
        if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
          await navigator.mediaDevices.getUserMedia({
              video: {
                width: { ideal: 300 },
                height: { ideal: 300 }
              },
              facingMode: { exact: "user" }
            }).then(stream => {
              this.video.srcObject = stream;
              this.video.play();
            });
        }
        window.requestAnimationFrame(this.loop);
      },
      async stop(){
        this.setStarted=false
        this.video.pause()
      },

      async loop() {
        // this.webcam.update(); // update the webcam frame
        await this.predict();
        window.requestAnimationFrame(this.loop);
      },

      async predict() {
        const prediction = await this.model.predict(this.video);
        this.downProb=prediction[0].probability
        this.upProb=prediction[1].probability
        
        if(this.upProb>.8){
          this.currentPosistion=='Up'?'':this.counter++
          this.currentPosistion='Up'
        }else if(this.downProb>.8){
          this.currentPosistion='Down'
        }
        // this.currentPosistion=prediction.className.Down>prediction.className.Up
        // prediction.className.Down>.85?

        // for (let i = 0; i < this.maxPredictions; i++) {
        //   const classPrediction =
        //     prediction[i].className + ": " + prediction[i].probability.toFixed(2);
        //   this.labelContainer.childNodes[i].innerHTML = classPrediction;
        //   // console.log(classPrediction)
        // }
      }
    }
  }
</script>
<style lang="scss" scoped>
.home{
  display: inline-flex;
  flex-wrap: wrap;
  border: 1px solid white;
  border-radius: 25px;
  position: relative;
  box-shadow: 0 0 1rem 0 rgba(0, 0, 0, .2); 
  z-index: 1;
  overflow: hidden;    
  &:before {
    content: "";
    position: absolute;
    background: inherit;
    z-index: -1;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    box-shadow: inset 0 0 60px rgba(255, 255, 255, .5);
    filter: blur(10px);
    margin: -20px;
}
  .MainUi{
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows:25px 1fr 30px;
    grid-column-gap: 0px;
    grid-row-gap: 0px;

    position: relative;
    box-shadow: 0 0 1rem 0 rgba(0, 0, 0, .2);   
    min-height: 200px;
    min-width: 300px;
    margin: 0 auto;

    h1{
      text-decoration: underline;
      text-align: left;
      padding-left:10px;
    }
    h2{
      font-size: 5rem;
      align-self: center;
      color: rgb(54, 133, 179);
    }
    .btn{
      width: 100%;
      bottom: 0;
      position: absolute;
      border-bottom-left-radius: 25px;
    }
  }
  #video{
    border-left: 1px solid white;
    margin: 0 auto;
    width: 300px;
    height: 300px;
    // border: 1px solid white;
  }
   @media only screen and (max-width: 602px) {
    .MainUi{
      width: 100%;
      h1{
         text-align: center;
      }
      .btn{
        border-bottom-left-radius: 0px;
      }
    }
    #video{
      border: none;
    }
   }
}
</style>