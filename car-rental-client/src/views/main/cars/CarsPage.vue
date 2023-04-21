<template>
  <HeaderSection />
  <div class="container py-5">
  <CardWidget>
        <div class="col gap-2 mb-5" v-for="car in cars" :key="car.car_id">
        <div class="card">
        <img :src="require('@/assets/uploads/cars/' + car.image)" class="card-img-top" :alt="car.brand" />
          <div class="card-body">
            <h5 class="card-title">{{ car.brand }}</h5>
            <p class="card-text">{{ car.model }}</p>
            <p class="card-text">year {{ car.year }}</p>
            <p class="card-text">{{ car.price }} $</p>
          </div>
          <div class="card-footer d-flex justify-content-center">
            <button class="btn btn-info px-4">More</button>
          </div>
        </div>
      </div>
    </CardWidget>
  </div>
  <FooterSection />
</template>

<script>
import HeaderSection from "@/components/HeaderSection.vue";
import FooterSection from "@/components/FooterSection.vue";
import CardWidget from "@/components/widgets/CardWidget.vue";
import axios from "axios";
import { ref, onMounted } from "vue";

export default { 
  components: { HeaderSection, FooterSection, CardWidget },
  setup() {
        const cars = ref([]);
        const error = ref(null);

    onMounted(async ()=> {
      try {
        const response = await axios.get('cars', { withCredentials: true });
        cars.value = response.data;
        console.log(response.data);
      }catch(err) {
        console.log(err);
      }

    });

    

    return { cars }

  },
  
  };
</script>

<style></style>
