<script>
import "bootstrap/dist/css/bootstrap.css";
import "font-awesome/css/font-awesome.css";

export default {
	name: "App",
	data: function() {
		return {
			prefixes: ["Air", "Jet", "Flight"],
			sufixes: ["Hub", "Station", "Mart"],
			domains: [
				"AirHub",
				"AirStation",
				"AirMart",
				"JetHub",
				"JetStation",
				"JetMart",
				"FligtHub",
				"FlightStation",
				"Flightmart"
			],
			prefix: '',
			sufix: ''
		};
	},
	methods: {
		addPrefix(prefix) {
			this.prefixes.push(prefix);
			this.prefix = '';
			this.generate();
		},
		addSufix(sufix) {
			this.sufixes.push(sufix);
			this.sufix = '';
			this.generate();
		},
		deletePrefix(prefix) {
			this.prefixes.splice((this.prefixes.indexOf(prefix)), 1);
			this.generate();
		},
		deleteSufix(sufix) {
			this.sufixes.splice((this.sufixes.indexOf(sufix)), 1);
			this.generate();
		},
		generate() {
			this.domains = [];
			for (const prefix of this.prefixes) {
				for (const sufix of this.sufixes) {
					this.domains.push(prefix + sufix);
				}
			}
		}
	}
};
</script>

<template>
  <div>
    <div id="slogan" class="text-center">
      <h1>NameGator</h1>
      <h6 class="text-secundary">Gerador de Nomes Utilizando Vue.js, GraphQL, Node.js</h6>
    </div>

    <div id="main">
      <div class="container">
        <div class="row">
          <div class="col-md">
            <h5>
              Prefixos
              <span class="badge badge-info">{{prefixes.length}}</span>
            </h5>
            <div class="card">
              <div class="card-body">
                <ul class="list-group">
                  <li class="list-group-item" v-for="prefix in prefixes" :key="prefix">
                    <div class="row">
                      <div class="col-md">
                        {{ prefix }}
                      </div>
                      <div class="col-md text-right">
                        <button class="btn btn-info" @click="deletePrefix(prefix)"> <span class="fa fa-trash"></span> </button>
                      </div>
                    </div>
                  </li>
                </ul>
                <br />
                <div class="input-group">
                  <input type="text" v-on:keyup.enter="addPrefix(prefix)" v-model="prefix" class="form-control" placeholder="Digite o Prefixo:" />
                  <div class="input-group-append">
                    <button @click="addPrefix(prefix)" class="btn btn-info">
                      <span class="fa fa-plus"></span>
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="col-md">
            <h5>
              Sufixos
              <span class="badge badge-info">{{sufixes.length}}</span>
            </h5>
            <div class="card">
              <div class="card-body">
                <ul class="list-group">
                  <li class="list-group-item" v-for="sufix in sufixes" :key="sufix">
                    <div class="row">
                      <div class="col-md">
                        {{ sufix }}
                      </div>
                      <div class="col-md text-right">
                        <button class="btn btn-info" @click="deleteSufix(sufix)"> <span class="fa fa-trash"></span> </button>
                      </div>
                    </div>
                  </li>
                </ul>
                <br />
                <div class="input-group">
                  <input type="text" v-on:keyup.enter="addSufix(sufix)" v-model="sufix" class="form-control" placeholder="Digite o Sufixo:" />
                  <div class="input-group-append">
                    <button @click="addSufix(sufix)" class="btn btn-info">
                      <span class="fa fa-plus"></span>
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <br />
        <h5>
          Domínios
          <span class="badge badge-info">{{ domains.length }}</span>
        </h5>
        <div class="card">
          <div class="card-body">
            <ul class="list-group">
              <li class="list-group-item" v-for="domain in domains" :key="domain">{{domain}}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
#slogan {
  margin-top: 30px;
  margin-bottom: 30px;
}
#main {
  background-color: #f1f1f1;
  padding-top: 30px;
  padding-bottom: 30px;
}
</style>
