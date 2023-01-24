<template>
  <v-app id="inspire">

    <v-app-bar>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Ecommerce</v-toolbar-title>
    </v-app-bar>

    <v-navigation-drawer
      v-model="drawer"
      temporary
    >
      <!--  -->
    </v-navigation-drawer>

    <v-main class="bg-grey-lighten-2">
      <v-container fluid> 
        <v-row>
          <v-col cols="12" md="2">
            <v-card class="py-2 px-2">
              <v-text-field placeholder="Search Product..." v-model="name"></v-text-field>
              <v-radio-group v-model="sortBy">
                <template v-slot:label>
                  <h3>Sort By</h3>
                </template>
                <v-radio value="name">
                  <template v-slot:label>
                    <div>By <strong>Name</strong></div>
                  </template>
                </v-radio>
                <v-radio value="price">
                  <template v-slot:label>
                    <div>By <strong>Price</strong></div>
                  </template>
                </v-radio>
              </v-radio-group>
              <v-radio-group v-model="order">
                <template v-slot:label>
                  <h3>Sorting By</h3>
                </template>
                <v-radio value="asending">
                  <template v-slot:label>
                    <div>By <strong>Asending</strong></div>
                  </template>
                </v-radio>
                <v-radio value="deasending">
                  <template v-slot:label>
                    <div>By <strong>Deasending</strong></div>
                  </template>
                </v-radio>
              </v-radio-group>
            </v-card>
          </v-col>
          <v-col cols="12" md="10">
            <v-row>
              <v-col v-for="(product, i) in filteredProducts" :key="i" cols="12" md="4" sm="6" lg="3">
                <v-card class="mx-auto">
                 <v-img
                 :src="product.image"
                 height="200px"
                 contain
               ></v-img>
                  <v-card-title>
                    {{ product.name }}
                  </v-card-title>
  
                  <v-card-subtitle>
                    {{ product.price }} FCFA
                  </v-card-subtitle>
  
                  <v-card-actions>
                    <v-btn
                      color="orange-lighten-2"
                      variant="text"
                    >
                      Read more
                    </v-btn>

                  </v-card-actions>
  
                </v-card>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup>
import data from './data'
const sortBy = ref("");
const order = ref("asending");
const products = ref(data);
const name=ref('');

const filteredProducts = computed(()=>{
  if (name.value) {
    return [...products.value].filter((item)=>{
      return name.value.toLocaleLowerCase().split(" ").every((v)=> item.name.toLocaleLowerCase().includes(v));
    })
  }else{
    return products.value;
  }

})

const sortProducts = ()=>{
  if (order.value== 'deasending') {
    products.value.sort(dynamicSort("-"+sortBy.value));
  }else{
    products.value.sort(dynamicSort(sortBy.value));
  }
}

const dynamicSort=(property)=>{
  let sortOrder = 1;
  if(property[0] === "-") {
      sortOrder = -1;
      property = property.substr(1);
  }
  return  (a, b) =>{
        /* next line works with strings and numbers, 
         * and you may want to customize it to your needs
         */
        const result = (a[property] < b[property]) ? -1 : (a[property] > b[property]) ? 1 : 0;
        return result * sortOrder;
    }
}

watch(sortBy, ()=>{
  sortProducts();
})
watch(order, ()=>{
  sortProducts();
})
</script>
