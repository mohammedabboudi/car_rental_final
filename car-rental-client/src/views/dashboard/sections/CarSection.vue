<template>
    <div class="d-flex justify-content-between pb-4">
        <h1>Our Cars</h1>
        <button class="btn btn-success new-car" data-bs-toggle="modal" data-bs-target="#addModal" data-bs-whatever="@mdo">New Car</button>
    </div>
    <h1 v-if="error" class="text-info">{{ error }}</h1>
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
          <div class="card-footer d-flex justify-content-between">
            <button class="btn btn-info" data-bs-toggle="modal" data-bs-target="#editModal" @click="showCarForEdit(car)">Edit</button>
            <button class="btn btn-danger" @click="deleteCar(car.car_id)">Delete</button>
          </div>
        </div>
      </div>
    </CardWidget>
    <ModalAddWidget>
        <form @submit.prevent="createNewCar" class="p-4" enctype="multipart/form-data">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">Add New Car</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                    <div class="mb-3">
                        <label for="brand" class="col-form-label">Brand:</label>
                        <input type="text" class="form-control" placeholder="Brand" v-model="brand" id="brand">
                    </div>
                    <div class="mb-3">
                        <label for="model" class="col-form-label">Model:</label>
                        <input type="text" class="form-control" placeholder="Model" v-model="model" id="model">
                    </div>
                    <div class="mb-3">
                        <label for="image" class="col-form-label">Image:</label>
                        <input type="file" class="form-control" placeholder="Image" @change="handleFileChange" id="image">
                    </div>
                    <div class="mb-3">
                        <label for="year" class="col-form-label">Year:</label>
                        <input type="text" class="form-control" placeholder="Year" v-model="year" id="year">
                    </div>
                    <div class="mb-3">
                        <label for="price" class="col-form-label">Price:</label>
                        <input type="number" class="form-control" placeholder="Price" v-model="price" id="price">
                    </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                <input type="submit" class="btn btn-primary" value="Create" data-bs-dismiss="modal">
            </div>
        </form>
    </ModalAddWidget>
    <ModalEditWidget>
        <form @submit.prevent="EditCar(carForEdit.car_id)" class="p-4" enctype="multipart/form-data">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">Edit Car</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                    <div class="mb-3">
                        <label for="brand" class="col-form-label">Brand:</label>
                        <input type="text" class="form-control" placeholder="Brand" v-model="carForEdit.brand" id="brand">
                    </div>
                    <div class="mb-3">
                        <label for="model" class="col-form-label">Model:</label>
                        <input type="text" class="form-control" placeholder="Model" v-model="carForEdit.model" id="model">
                    </div>
                    <div class="mb-3">
                        <label for="image" class="col-form-label">Image:</label>
                        <input type="file" class="form-control" placeholder="Image" @change="handleFileChange" id="image">
                    </div>
                    <div class="mb-3">
                        <label for="year" class="col-form-label">Year:</label>
                        <input type="text" class="form-control" placeholder="Year" v-model="carForEdit.year" id="year">
                    </div>
                    <div class="mb-3">
                        <label for="price" class="col-form-label">Price:</label>
                        <input type="number" class="form-control" placeholder="Price" v-model="carForEdit.price" id="price">
                    </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                <input type="submit" class="btn btn-primary" value="Edit" data-bs-dismiss="modal">
            </div>
        </form>
    </ModalEditWidget>
    
</template>

<script>
import CardWidget from '@/components/widgets/CardWidget.vue';
import ModalAddWidget from '@/components/widgets/ModalAddWidget.vue';
import ModalEditWidget from '@/components/widgets/ModalEditWidget.vue';
import axios from 'axios';
import { ref, onMounted, reactive } from 'vue';
export default {
    components: { CardWidget, ModalAddWidget, ModalEditWidget },
    setup() {
        const cars = ref([]);
        const error = ref(null);

        // ADD FORM FIELDS :

        const brand = ref('');
        const model = ref('');
        const year = ref('');
        const price = ref('');
        
        //
        const image = ref(null);
        //
        
        // EDITED CAR :

        const carForEdit = reactive({});
        
        onMounted(async ()=> {
            try {
                const response = await axios.get('cars', { withCredentials: true });
                cars.value = await response.data;
            }catch(err) {
                error.value = 'no data available for the moment!'
            }

        });

        const createNewCar = async () => {
            const formData = new FormData();
            formData.append('brand', brand.value);
            formData.append('model', model.value);
            formData.append('image', image.value);
            formData.append('year', year.value);
            formData.append('price', price.value);
            try {
                const response = await axios.post('cars', formData, { withCredentials: true});
                location.reload();
                
            } catch (error) {
                console.log(error);
            }
        }

        const handleFileChange = (event) => {
            return image.value = event.target.files[0];
        }


        const showCarForEdit = (car) => {
            return Object.assign(carForEdit, car);
        }

        const EditCar = async (car_id) => {
            const formData = new FormData();
            if(image.value != null) {
                    formData.append('newImage', image.value);
                    formData.append('image', carForEdit.image);
            }

            formData.append('brand', carForEdit.brand);
            formData.append('model', carForEdit.model);
            formData.append('year', carForEdit.year);
            formData.append('price', carForEdit.price);
            try {
                const response = axios.put(`cars/${car_id}`, formData, { withCredentials: true});
                location.reload();
            } catch (error) {
                console.log(error);
            }
        }


        const deleteCar = async (car_id) => {
        try {
          const response = axios.delete(`cars/${car_id}`, { withCredentials: true });
          console.log(response);
          location.reload();
        } catch (error) {
          console.log(error);
        }
      }

        return { createNewCar, handleFileChange, showCarForEdit, deleteCar, EditCar, cars, error, brand, model, image, year, price, carForEdit }
    }
}
</script>

<style></style>