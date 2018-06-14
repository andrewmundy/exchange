<template>
  <div id="app">
    <div id="container">
      <img class="logo" src="./logo.png">
      <div class="inputs">
        <div>
          <span>$</span>
          <input
            onclick="document.getElementById('from').value = ''"
            id="from"
            type="tel"
            v-model="fromValue"    
            onfocus="if(this.value == 'value') { this.value = ''; }"
          >
          <select v-model="fromCountryCurrency">
            <option>{{fromCountryCurrency}}</option>
            <option value="USD">United States dollar</option>
            <option value="IDR">Indonesian rupiah</option>
            <option value="THB">Thai baht</option>
            <option value="LAK">Lao kip</option>
            <option value="GBP">British pound</option>
            <option value="EUR">European euro</option>
          </select>
        </div>

        <div>
          <span>$</span>
          <input
            v-model="toValue"
          >
          <select v-model="toCountryCurrency">
            <option>{{toCountryCurrency}}</option>
            <option value="USD">United States dollar</option>
            <option value="IDR">Indonesian rupiah</option>
            <option value="THB">Thai baht</option>
            <option value="LAK">Lao kip</option>
            <option value="GBP">British pound</option>
            <option value="EUR">European euro</option>
          </select>
        </div>
      </div>
      <span class="from">📍 {{fromCountryName}}</span>
      <div id="map"></div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  data(){
    return{
      latitude:0,
      longitude:0,
      rate:0,
      initializing:true,
      fromCountryCurrency:'',
      fromCountryName:'',
      fromCountryCode:'',
      fromValue:0,
      toCountryName:'',
      toCountryCode:'US',
      toCountryCurrency:'USD',
      toValue:1,
      located:false
    }
  },
  created(){
    this.findLocation()
    this.toCountryCurrency = localStorage.getItem('toCountry')
  },
  mounted(){
    },
  updated(){
    
    },
  watch:{
    fromValue:function(){
      this.fromMath()
      // this.toValue = "$" + this.toValue
      // this.fromValue = "$" + this.fromValue
    },
    toValue:function(){
      // this.toMath()
    },
    fromCountryCurrency:function(){
      this.toValue = 1
      // this.fromValue = 1
      this.countryToRate("change")
    },
    toCountryCurrency:function(){
      // this.fromValue = 0
      this.countryFromCountryCode()
      let store = this.toCountryCurrency
      localStorage.setItem('toCountry', store)
      console.log(localStorage.getItem('toCountry'))
    },
    located:function(){
      this.createMap()
    }
  },
  methods:{
    findLocation(){
      let that = this
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(function(position) {
          that.latitude = position.coords.latitude
          that.longitude = position.coords.longitude
          that.located = true
        })
      }
    },
    createMap(){
      let that = this
      let url = "https://maps.googleapis.com/maps/api/geocode/json?latlng=" + that.latitude + "," + that.longitude + "&result_type=country&key=AIzaSyAc9BvmSaga2NJwzDn7iSn_Oz6I7Th3oIE"

      fetch(url)
        .then((resp) => resp.json())
        .then(function(data){
          that.fromCountryName = data.results[0].address_components[0].long_name
          that.fromCountryCode = data.results[0].address_components[0].short_name
          that.coordToCountry()
        })      
    },
    coordToCountry(){
      let that = this
      let img = new Image()
      img.src = "https://maps.googleapis.com/maps/api/staticmap?center=" + that.latitude + "," + that.longitude + "&zoom=13&size=400x120&sensor=false&key=AIzaSyAc9BvmSaga2NJwzDn7iSn_Oz6I7Th3oIE"
      map.appendChild(img)
      that.countryFromCountryCode()
    },
    countryFromCountryCode(){
    let that = this
    let countries = "https://www.currencyconverterapi.com/api/v5/countries?apiKey=d996c6d6-ec4a-46d0-ad17-f9eba3092eb9"

    fetch(countries)
      .then((resp) => resp.json())
      .then(function(data){
        // console.log(data.results[that.fromCountryCode].currencyName)
        // that.fromCountryCurrency = data.results[that.fromCountryCode].currencyName
        that.fromCountryCurrency = data.results[that.fromCountryCode].currencyId
        that.countryToRate()
      })
    },
    countryToRate(...args){
      let that = this
      let from = that.fromCountryCurrency
      let to = that.toCountryCurrency
      let convert = "https://www.currencyconverterapi.com/api/v5/convert?q="+to+"_"+from+"&compact=ultra&apiKey=d996c6d6-ec4a-46d0-ad17-f9eba3092eb9"
      console.log(convert)

      fetch(convert)
        .then((resp) => resp.json())
        .then(function(data){
          that.rate = data[to+"_"+from]
          console.log("loaded rate " + that.rate)
          // that.fromValue = that.rate
          that.popFromValue()
        })

      // that.toValue = that.fromValue * that.rate.toFixed(2)
    },
    popFromValue(){
      this.fromValue = (this.toValue * this.rate).toFixed(2)
    },
    fromMath(){
      this.toValue = (this.fromValue / this.rate).toFixed(2)
    },
    toMath(){
      this.fromValue = (this.toValue * this.rate).toFixed(2)
    }
  }
}
</script>

<style lang="scss">
body {
  margin:0px;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  background:#8C9AFE;
  display:flex;
  justify-content: center;
  align-content: center;
}
#container{
  box-shadow: 0px 20px 70px #0002;
  text-align: center;
  background:white;
  width:400px;

  .logo{
    width:50px;
    padding:1rem;
  }
  .inputs{
    padding:1rem;
    font-size:14px;
    span{
      font-size: 12px;
    }
    *{
      margin:2px;
    }
  }
  #map{
    height: 120px;
  }
  .from{
    color:grey;
  }
}
</style>
