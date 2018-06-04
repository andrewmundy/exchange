<template>
  <div id="app">
      <!-- <img class="bg" src="./bg2.svg">
      <img class="bg2" src="./bg1.svg"> -->
      <div id="container">
        <div class="info">

          <div class="inputs">
            Hi 👋 you are currently in {{fromCountryName}}
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
        fromCountry:'USD',
        fromCountryName:'',
        fromCountryFlag:'',
        fromCountryCode:'',
        toValue:0,
        toCountry:'THB',
        toCountryName:'',
        toCountryFlag:'',
        toCountryCode:'',
				value: 'hello',
        rate:100,
        lat:'',
        long:''
			}
    },
    mounted:function(){
      var https = require('https')
      var options = {
        enableHighAccuracy: true,
        timeout: 5000,
        maximumAge: 0
      };

      function success(pos) {
        var crd = pos.coords;

        alert('Your current position is:');
        alert(`Latitude : ${crd.latitude}`);
        alert(`Longitude: ${crd.longitude}`);
        alert(`More or less ${crd.accuracy} meters.`);
      }

      function error(err) {
        alert(`ERROR(${err.code}): ${err.message}`);
      }

      navigator.geolocation.getCurrentPosition(success, error, options);

      this.location()
      // this.money()
    },
    updated:function(){
    },
    methods:{
      // math(){
      //   let that = this

      //   //sample data
      //   that.fromValue = 
      //   that.toValue = 
      //   that.rate = 

      //   that.fromValue * that.rate
      // },
      money: function(){
        let from = this.fromCountry
        let to = this.toCountry
        
        let convert = "https://www.currencyconverterapi.com/api/v5/convert?q="+from+"_"+to+"&compact=ultra&apiKey=d996c6d6-ec4a-46d0-ad17-f9eba3092eb9"
        let countries = "https://www.currencyconverterapi.com/api/v5/countries?apiKey=d996c6d6-ec4a-46d0-ad17-f9eba3092eb9"
        let that = this
        fetch(countries)
          .then((resp) => resp.json())
          .then(function(data){
            that.fromCountry = data.results[that.fromCountryCode].currencyId
            console.log(that.fromCountry)
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
        // const ip = '134.201.250.155'
        // const access_key = 'e1f8abf46f10cca826e52a483d711dac'
        // const url = 'http://api.ipstack.com/' + ip + '?access_key=' + access_key
        alert(that.toValue)
        if (navigator.geolocation) {
          navigator.geolocation.getCurrentPosition(function(position) {
            that.lat = position.coords.latitude
            that.long = position.coords.longitude
            let url = "https://maps.googleapis.com/maps/api/geocode/json?latlng=" + that.lat + "," + that.long + "&result_type=country&key=AIzaSyAc9BvmSaga2NJwzDn7iSn_Oz6I7Th3oIE"
            // let url = "https://maps.googleapis.com/maps/api/geocode/json?latlng=14.728205,102.224386&result_type=country&key=AIzaSyAc9BvmSaga2NJwzDn7iSn_Oz6I7Th3oIE"
            // console.log(url)

          fetch(url)
            .then((resp) => resp.json())
            .then(function(data){
              that.fromCountryName = data.results[0].address_components[0].long_name
              that.fromCountryCode = data.results[0].address_components[0].short_name
              console.log(that.fromCountryName)
            })
          // console.log(latitude + " " + longitude)
          // let img = new Image();
          // img.src = "https://maps.googleapis.com/maps/api/staticmap?center=" + latitude + "," + longitude + "&zoom=13&size=400x120&sensor=false";
          // map.appendChild(img);
        },function(error){
          alert("oops timeout")
        },{timeout:4000})
        }
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
  height:300px;
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

