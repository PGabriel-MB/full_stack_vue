<template>
  <div>
    <h5>
      {{ title }}
      <span class="badge badge-info">{{ items.length }}</span>
    </h5>
    <div class="card">
      <div class="card-body">
        <ul class="list-group">
          <li class="list-group-item" v-for="item in items" :key="item.id">
            <div class="row">
              <div class="col-md">{{ item.description }}</div>
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
            v-model="description"
            class="form-control"
            placeholder="Digite o Item:"
          />
          <div class="input-group-append">
            <button @click="addItem(type, description)" class="btn btn-info">
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
	props: ["items", "type", "title"],
	data() {
		return {
			description: "",
		};
	},
	methods: {
		addItem(type, description) {
			this.$emit("addItem", {
				type,
				description,
			});
			this.description = "";
		},
		deleteItem(item) {
			this.$emit("deleteItem", item);
		},
	},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
