<template>
  <div id="app">
    <div>
      <div id="container">
        <div class="contents">
          <div class="inputs">
          {{total()}}

            <input
              v-bind:placeholder="this.country"
              v-model="us"
            >
            <input
              v-model="exchange"
              v-on:input="total"            
            >
          </div>
          <div id="output"></div>
          <!-- <div class="bottom" id="output">
            <div>exchange</div>
            <div>stuff</div>
          </div> -->
        </div>
    </div>
  </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data: function(){
			return{
				value: 'hello',
				us:"",
				rate:"",
        exchange:'',
        country:"THB"
			}
    },
    mounted:function(){
      this.location()
      this.money()
      this.assignCountry()
      // this.total()
    },
    methods:{
      assignCountry: function(){
        this.country = "THB"
      },
      money: function (){
        this.us = "$" + this.us
      },
      total: function(){
        this.exchange = "$" + Math.round(this.us * this.rate) + " 🇺🇸"
      },
      money: function(){
        let url = "http://data.fixer.io/api/latest?access_key=c98b600783e240ffe856deaaa0747493"
        let that = this
        fetch(url)
          .then((resp) => resp.json())
          .then(function(data){
            that.rate = data.rates[that.country]*0.001
          })
      },
      location: function(){
        const ip = '134.201.250.155'
        const access_key = 'e1f8abf46f10cca826e52a483d711dac'
        const url = 'http://api.ipstack.com/' + ip + '?access_key=' + access_key
        if ("geolocation" in navigator) {
          navigator.geolocation.getCurrentPosition(function(position) {
          let latitude = position.coords.latitude
          let longitude = position.coords.longitude
          var img = new Image();
          img.src = "https://maps.googleapis.com/maps/api/staticmap?center=" + latitude + "," + longitude + "&zoom=13&size=300x300&sensor=false";
          output.appendChild(img);
        })
        } else {
          console.log("oops")
        }

        fetch(url)
          .then((resp) => resp.json())
          .then(function(data){
            console.log(data.country_name)
          })
      }
    }
  }
</script>

<!-- CSS libraries -->
<style src="normalize.css/normalize.css"></style>

<!-- Global CSS -->
<style>
  .bottom{
    background:white;
    display: flex;
    justify-content: space-around;
    align-items: center;
    width:100vw;
    position:fixed;
    bottom:0;
  }
  .bottom *{
    background: white;
    color: black;
    width: 25%;
    padding: 1rem 0;
    box-shadow: 0px 10px 20px #0000001a;
    border-radius: 10px;
  }
  img{
    /* border-style:solid; */
    border-radius:100rem;
    padding:1rem;
  }
  img #output{
  }
  h1{
    padding:1rem;
    color:white !important;
    font-size:3rem;
  }
  body{
    font-family:sans-serif;
    color:white;
    background-image: linear-gradient(120deg, #e0c3fc 0%, #8ec5fc 100%);
    height:100vh;
  }
  input{
    font-size:3rem;
    border:none;
    background:rgba(255,255,255,0.2);
    padding:1rem;
    margin:1rem;
    border-radius:5px;
    width:20%;
    color:white;
  }
  input::placeholder{
      color:white;
  }
  .inputs{
    display:flex;
    justify-content:center;
  }
  .contents{
    display:flex;
    justify-content:center;
    align-items:center;
    flex-direction:column;
  }
  #container{
    display:flex;
    justify-content:center;
  }
  code {
    font-family: Menlo, Monaco, Lucida Console, Liberation Mono, DejaVu Sans Mono, Bitstream Vera Sans Mono, Courier New, monospace, serif;
    font-size: 0.9em;
    white-space: pre-wrap;
    color: #2c3e50;
  }

  code::before, code::after {
    content: '`';
  }
</style>

<!-- Scoped component css -->
<!-- It only affect current component -->
<style scoped>
  #app {
    text-align: center;
  }

  #app h1 {
    color: #2c3e50;
    font-weight: 300;
    margin: 0;
  }

  .banner {
    height: 200px;
    background-color: #f6f6f6;
    padding: 50px 10px;
  }

  .bottom {
    padding: 80px 10px;
    font-size: 24px;
    font-weight: 300;
  }

  .fade {
    font-size: 14px;
  }

  .logo {
    animation: spin 4s 1s infinite linear
  }

  @keyframes spin {
    from {transform:rotate(0deg);}
    to {transform:rotate(360deg);}
  }
</style>
