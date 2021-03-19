<template>
<div class="box">
    <h2 v-if="typeBox === 'state'">Casos por estado</h2>
    <h2 v-if="typeBox === 'countrie'">Casos por país</h2>
    <hr>
    <h5 class="orange">{{name}}</h5>
    
    <select id="state" name="state" v-if="typeBox === 'state'" @change="getCases($event)">        <option disabled value="" selected>Selecione</option>
        <option v-for="item in states" :key="item" :value="item">{{ item }}</option>
    </select>

    <select name="countrie" id="countrie" v-if="typeBox === 'countrie'" @change="getCasesByCountrie($event)">
        <option disabled value="" selected>Selecione</option>
        <option v-for="item in countries" :key="item" :value="item">{{ item }}</option>
    </select>
    
    <p>Casos confirmados: <span class="confirmed">{{cases}}</span></p>
    <p>Recuperados: <span class="recovered">{{recovered}}</span></p>
    <p>Mortos: <span class="dead">{{deaths}}</span></p>
</div>
</template>

<script>
export default {
name: "Box",
props: ["typeBox"],
data() {
    return {
        cases: 0,
        deaths: 0,
        recovered: 0,
        name: "Nenhum",
        countries: [],
        states: []
    };
},
created() {
    this.getCountries(),
    this.getStates()
},
    methods: {
        getCases:function(event) {
            var uf = event.target.value.toString();

            let url = "https://covid19-brazil-api.now.sh/api/report/v1/brazil/uf/" + uf.toLowerCase();
            
            fetch(url).then(res => {
                return res.json();
            }).then(json => {
                this.cases = parseInt(json.cases).toLocaleString('pt-BR');
                this.deaths = parseInt(json.deaths).toLocaleString('pt-BR');
                this.recovered = parseInt(json.cases - json.deaths).toLocaleString('pt-BR');
                this.name = json.state;
            })
        },

        getCasesByCountrie:function(event) {
            var countrie = event.target.value.toString();

            let url = "https://covid19-brazil-api.now.sh/api/report/v1/" + countrie.toLowerCase();
            
            fetch(url).then(res => {
                return res.json();
            }).then(json => {
                this.cases = parseInt(json.data.confirmed).toLocaleString('pt-BR');
                this.deaths = parseInt(json.data.deaths).toLocaleString('pt-BR');
                this.recovered = parseInt(json.data.recovered).toLocaleString('pt-BR');
                this.name = json.data.country;
            })
        },

        getCountries() {
            let url = "https://covid19-brazil-api.now.sh/api/report/v1/countries";

            fetch(url).then(res => {
                return res.json();
            }).then(json => {
                
                for (var i=0;i<json.data.length;i++){
                    this.countries.push(json.data[i].country);
                }
            })
        },

        getStates() {
            let url = "https://covid19-brazil-api.now.sh/api/report/v1";

            fetch(url).then(res => {
                return res.json();
            }).then(json => {
                for (var i=0;i<json.data.length;i++){
                    this.states.push(json.data[i].uf);
                }
            })
        }
    }
}

</script>

<style scoped>
.box {
    max-width: 300px;
    padding: 20px 40px 20px 40px;
    box-shadow: 0px 4px 8px 0px rgba(0,0,0,0.2);
    margin: 5px;
}

.box h2 {
    color: #313131;
}

.orange {
    background: linear-gradient(to top right,#ff2a00 0,#ffb600 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    font-size: 15px;
}

.box select {
  padding: 5px 10px 5px 0px;
  color: #313131;
  font-size: 15px;
  appearance: none;
  outline: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  transition: 0.4s;
  border: 1px solid black;
  margin-bottom: 20px;
  background: url(http://www.webcis.com.br/images/imagens-noticias/select/ico-seta-appearance.gif) no-repeat #eeeeee;
  background-position: 270px center;  /*Posição da imagem do background*/   
  width: 300px;
   /* Tamanho do select, maior que o tamanho da div "div-select" */   
   height:40px; /* Altura do select, importante para que tenha a mesma altura em todo os navegadores */   
}

.box p {
    margin: 0;
    padding: 0;
    text-align: start;
    font-weight: bold;
}

.confirmed {
    color: blue;
}

.recovered {
    color: green;
}

.dead {
    color: red;
}
</style>