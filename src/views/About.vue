<template>
  <div class="about">
    <!-- <img src="../assets/pushUp.jpg" alt=""> -->
  <div class="content">
    <h1>Motivation</h1>
    <p>I'm really bad at counting when I do exercises, and with Covid-19 causing the gyms to close i've found myself doing a lot more push ups. So i figured i'd have technology do the counting so i can really focus on my mind to muscle connection</p>
  </div>
  <div class="content">
    <h1>How To Use</h1>
    <p>Just get in view of your camera and start! (Best results when facing directly torwards the camera)</p>
  </div>
  </div>
</template>
<style lang="scss" scoped>
.about{
  width: 100%;
  height: 100%;
  min-height: 80vh;
  text-align: left;
  background-size: cover;
  background-image: url('../assets/pushUp.jpg');
  display: inline-flex;
  flex-direction: row;
  flex-wrap: wrap;
  .content{
    margin: 10px;
    width: 320px;
    height: 100%;
    box-shadow: 0 0 1rem 0 rgba(0, 0, 0, .2);   
    border-radius: 5px;
    position: relative;
    z-index: 1;
    overflow: hidden;  
    h1{
      padding-left: 4px;
    }
    p{
      padding: 5px;
    }
    &:before {
    content: "";
    position: absolute;
    background: inherit;
    z-index: -1;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    box-shadow: inset 0 0 2000px rgba(255, 255, 255, .5);
    filter: blur(10px);
    margin: -20px;
}
  }

}
</style>