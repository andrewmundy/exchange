<template>
  <div id="app">
      <!-- <img class="bg" src="./bg2.svg">
      <img class="bg2" src="./bg1.svg"> -->
      <div id="container">
        <div class="info">
          <div class="inputs">
              <input 
              onclick="document.getElementById('from').value = ''"
                id="from"
                type="tel"
                v-model="fromValue"    
                onfocus="if(this.value == 'value') { this.value = ''; }"
              >
              <select v-model="fromCountry">
                <option>{{fromCountry}}</option>
                <option>USD</option>
                <option>THB</option>
                <option>LKP</option>
              </select>
          </div>

          <div class="contents">
            <div class="inputs">
              <input
                type="number"
                min="0"
                step="any"
                v-model="toValue"
              >
              <select v-model="toCountry">
                <option>THB</option>
                <option>USD</option>
                <option>LAK</option>
              </select>

            </div>
          
          </div>

        </div>
            <!-- <span>{{fromCountryFlag}}</span> -->
          <div id="map"></div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data: function(){
			return{
        fromValue:1,
        fromCountry:"USD",
        fromCountryName:"",
        fromCountryFlag:"",
        fromCountryCode:"",
        toValue:0,
        toCountry:"THB",
        toCountryName:"",
        toCountryFlag:"",
        toCountryCode:"",
				value: 'hello',
        rate:100,
			}
    },
    mounted:function(){
      this.location()
      // this.money()
    },
    updated:function(){
    },
    methods:{
      math(){
        let that = this

        //sample data
        that.fromValue = 
        that.toValue = 
        that.rate = 

        that.fromValue * that.rate
      },
      money: function(){
        var https = require('https')
        let from = this.fromCountry
        let to = this.toCountry
        
        let convert = "https://www.currencyconverterapi.com/api/v5/convert?q="+from+"_"+to+"&compact=ultra&apiKey=d996c6d6-ec4a-46d0-ad17-f9eba3092eb9"
        let countries = "https://www.currencyconverterapi.com/api/v5/countries?apiKey=d996c6d6-ec4a-46d0-ad17-f9eba3092eb9"
        let that = this
        fetch(countries)
          .then((resp) => resp.json())
          .then(function(data){
            that.fromCountry = data.results[that.fromCountryCode].currencyId
            // console.log(that.fromCountry)
          })
        fetch(convert)
          .then((resp) => resp.json())
          .then(function(data){
            console.log(data)
            that.rate = data[from+"_"+to]
            that.toValue = that.fromValue * that.rate
          })
        that.toValue = that.fromValue * that.rate.toFixed(2)
        console.log(that.toValue)
      },
      location: function(){
        let that = this
        const ip = '134.201.250.155'
        const access_key = 'e1f8abf46f10cca826e52a483d711dac'
        const url = 'https://api.ipstack.com/' + ip + '?access_key=' + access_key
        if ("geolocation" in navigator) {
          navigator.geolocation.getCurrentPosition(function(position) {
          let latitude = position.coords.latitude
          let longitude = position.coords.longitude
          var img = new Image();
          img.src = "https://maps.googleapis.com/maps/api/staticmap?center=" + latitude + "," + longitude + "&zoom=13&size=400x120&sensor=false";
          map.appendChild(img);
        })
        } else {
          console.log("oops")
        }

        fetch(url)
          .then((resp) => resp.json())
          .then(function(data){
            that.fromCountryName = data.country_name
            that.fromCountryCode = data.country_code
            that.fromCountryFlag = data.location.country_flag_emoji

            // console.log(data)
          })
      }
    }
  }
</script>

<!-- CSS libraries -->
<style type="text/scss">
body{
  font-family: Arial, Helvetica, sans-serif;
  /* background-image: linear-gradient(to top, #00c6fb 0%, #005bea 100%); */
  background:#8C9AFE;
  height:100vh;
}
.info{
  padding:2rem;
}

.inputs input{
  width:auto;
  font-size: 1rem;
}
select{
  font-size:1rem;
}
.legend div{
  margin:auto;
  width: 50%;
  display:flex;
  justify-content: space-between;
  color:grey;
}
.bg2{
  opacity: 0.4;
  right: 0;
  top: -800px;
  width: 60vw;
  position: fixed;
  z-index: 0;
}
.bg{
  opacity:0.4;
  left: 0;
  top: -100px;
  width: 60vw;
  position: fixed;
  z-index: 0;
}
#map{
  height: 120px;
}
#map > img{
  border-radius: 0 0 10px 10px;
  margin-top:1rem;

}
.inputs{
text-align: left;
  padding:0.2rem;
}
#app{
  display:flex;
  justify-content: center;
  align-content: center;
}
#container{
  width:400px;
  text-align: center;
  background: white;
  box-shadow: 0px 20px 70px #0006;
  /* padding: 3rem 2rem 0 2rem; */
  border-radius: 0 0 10px 10px;
  position: fixed;
  top:0;
}
</style>

