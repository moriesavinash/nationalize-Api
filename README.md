<body>

  <h1>Nationalize API</h1>
  <div id="main-container">
    <div id="flag-container">
      <img src="" alt="">
    </div>
    <div id="info-container">
      <select id="countries"></select>
      <p>Capital: <country id="name"></country></p>
      
      <p>country: <country id="country"></country></p>
      
      <p>probability: <country id="probability"></country></p>
    
    </div>
  </div>

</body>
* {
  box-sizing: border-box;
}
body {
  font-family: Helvetica, Arial, sans-serif;
  font-size: 15px;
  color: #333;
  background-color: #eee;
}
h1 {
  text-align: center;
}
#main-container {
  width: 502px;
  margin: 30px auto;
  padding: 0;
}
#flag-container {
  height: 252px;
  background-color: #fff;
  border: 1px solid #333;
}
#flag-container img {
  display: block;
  width: 100%;
  height: 100%;
}
#info-container select {
  display: block;
  margin: 20px auto;
  padding: 5px;
  min-width: 100%;
  color: #333;
  font-size: 15px;
  font-weight: 900;
  text-align-last: center;
}
#info-container p {
  padding: 0 10px;
  font-weight: 600;
}
#info-container p span {
  font-weight: normal;
}

@media (max-width: 768px) {
  body { font-size: 12px; }
  #main-container { width: 342px; }  
  #flag-container { height: 172px; }  
  #info-container select { font-size: 12px; font-weight: 600; }
}

const countriesList = document.getElementById("countries");
let countries; 



countriesList.addEventListener("change", newCountrySelection);

function newCountrySelection(event) {
  displayCountryInfo(event.target.value);
}



fetch("https://api.nationalize.io/?name[]=michael&name[]=matthew&name[]=jane")
.then(res => res.json())
.then(data => initialize(data))
.catch(err => console.log("Error:", err));

function initialize(countriesData) {
  countries = countriesData;
  let options = "";
  
  function resolveAfter2Seconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('name');
    }, 2000);
  });
}

async function asyncCall() {
  console.log('country');
  const result = await resolveAfter2Seconds();
  console.log(result);
  
}

asyncCall();

  
  
  
  
  countries.forEach(country => options+=`<option value="${country.alpha3Code}">${country.name}</option>`);
  
  countriesList.innerHTML = options;
  
  countriesList.selectedIndex = Math.floor(Math.random()*countriesList.length);
  displayCountryInfo(countriesList[countriesList.selectedIndex].value);
}

function displayCountryInfo(countryByAlpha3Code) {
  const countryData = countries.find(country => country.alpha3Code === countryByAlpha3Code);
  
  document.getElementById("name").innerHTML = countryData.currencies.filter(c => c.name).map(c => `${c.name} (${c.code})`).join(", ");
  document.getElementById("country").innerHTML = countryData.region;
  document.getElementById("probability").innerHTML = countryData.subregion;
}
