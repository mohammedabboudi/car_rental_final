<template>
  <div class="fleetSection pb-5">
    <h1 class="text-success fs-2 fw-semibold mb-5">Our Fleet is Your Fleet</h1>
    <CardWidget>
      <div class="col gap-2 mb-5" v-for="brand in brands" :key="brand.brand_id">
        <div class="card">
        <img :src="require('@/assets/uploads/brands/' + brand.image)" class="card-img-top" :alt="brand.name" />
          <div class="card-body">
            <h5 class="card-title">{{ brand.name }}</h5>
            <p class="card-text">{{ brand.description }}</p>
          </div>
          <div class="card-footer d-flex justify-content-center">
            <button class="btn btn-info px-4" @click="showBrandCars(brand.name)">More</button>
          </div>
        </div>
      </div>
    </CardWidget>
  </div>
</template>

<script>
import axios from "axios";
import { ref, onMounted } from "vue";
import CardWidget from "./widgets/CardWidget.vue";
export default {
  components: { CardWidget },
  setup() {
        const brands = ref([]);
        const error = ref(null);

    onMounted(async () => {
      const response = await axios.get('brands', { withCredentials: true });
      brands.value = response.data;

      console.log(response.data)

    });

    const showBrandCars = (brand_name) => {
      console.log(brand_name)
    }
    
    return { brands, showBrandCars }

  }
};
</script>

<style></style>
