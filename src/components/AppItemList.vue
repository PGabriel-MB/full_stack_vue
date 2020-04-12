<template>
  <div>
    <h5>
			{{ title }}
      <span class="badge badge-info">{{items.length}}</span>
    </h5>
    <div class="card">
      <div class="card-body">
        <ul class="list-group">
          <li class="list-group-item" v-for="item in items" :key="item">
            <div class="row">
              <div class="col-md">{{ item }}</div>
              <div class="col-md text-right">
                <button class="btn btn-info" @click="deleteItem(item)">
                  <span class="fa fa-trash"></span>
                </button>
              </div>
            </div>
          </li>
        </ul>
        <br />
        <div class="input-group">
          <input
            type="text"
            v-on:keyup.enter="addItem(item)"
            v-model="item"
            class="form-control"
            placeholder="Digite o Item:"
          />
          <div class="input-group-append">
            <button @click="addItem(item)" class="btn btn-info">
              <span class="fa fa-plus"></span>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
	name: "AppItemList",
	props: ["items", "title"],
	data () {
		return {
			item: ""
		};
	},
	methods: {
		addItem(item) {
			this.$emit('addItem', item);
			this.item = "";
		},
		deleteItem(item) {
			this.$emit('deleteItem', item);
		}
	}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
