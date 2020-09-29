<script>
import "bootstrap/dist/css/bootstrap.css";
import "font-awesome/css/font-awesome.css";
import AppItemList from "./AppItemList";
import axios from "axios/dist/axios";

export default {
	name: "App",
	components: {
		AppItemList
	},
	data: function() {
		return {
			items: {
				prefix: [],
				suffix: []
			}
		};
	},
	methods: {
		addPrefix(prefix) {
			axios({
				url: 'http://localhost:4000',
				method: 'post',
				data: {
					query: `
						mutation ($item: ItemInput) {
							newPrefix: saveItem(item: $item) {
								id
								type
								description
							}
						}
					`,
					variables: {
						item: {
							type: 'prefix',
							description: prefix
						}
					}
				}
			}).then(response => {
				const query = response.data;
				const newPrefix = query.data.newPrefix;
				this.items.prefix.push(newPrefix);
			});
		},
		addSufix(sufix) {
			this.sufixes.push(sufix);
		},
		deletePrefix(prefix) {
			axios({
				url: 'http://localhost:4000',
				method: 'post',
				data: {
					query:`
						mutation ($id: Int){
							deleted: deleteItem(id: $id)
						}
					`,
					variables: {
						id: prefix.id
					}
				}
			}).then(() => {
				this.getItems('prefix');
			});
		},
		deleteSufix(sufix) {
			this.sufixes.splice(this.sufixes.indexOf(sufix), 1);
		},
		getItems(type) {
			axios({
				url: "http://localhost:4000",
				method: "post",
				data: {
					query: `
						query ($type: String){
							items: items (type: $type) {
							  id
							  type
							  description
							}
						}
					`,
					variables: {
						type
					}
				}
			}).then(response => {
				const query = response.data;
				this.items[type] = query.data.items;
			});
		}
	},
	computed: {
		domains() {
			const domains = [];
			for (const prefix of this.items.prefix) {
				for (const suffix of this.items.suffix) {
					const name = prefix.description + suffix.description;
					const url = name.toLowerCase();
					const checkout = `https://checkout.hostgator.com.br/?a=add&sld=${url}&tld=.com`;
					domains.push({
						name,
						checkout
					});
				}
			}
			return domains;
		}
	},
	created() {
		this.getItems("prefix");
		this.getItems("suffix");
	}
};
</script>

<template>
  <div>
    <div id="main">
      <div class="container">
        <div class="row">
          <div class="col-md">
            <AppItemList
              title="Prefixos"
              :items="items.prefix"
              @addItem="addPrefix"
              @deleteItem="deletePrefix"
            ></AppItemList>
          </div>
          <div class="col-md">
            <AppItemList
              title="Sufixos"
              :items="items.suffix"
              @addItem="addSufix"
              @deleteItem="deleteSufix"
            ></AppItemList>
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
              <li class="list-group-item" v-for="domain in domains" :key="domain.name">
                <div class="row">
                  <div class="col-md">{{domain.name}}</div>
                  <div class="col-md text-right">
                    <a class="btn btn-info" :href="domain.checkout" target="_blank">
                      <span class="fa fa-shopping-cart"></span>
                    </a>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
</style>
