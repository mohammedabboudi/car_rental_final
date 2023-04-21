<template>
    <div class="d-flex justify-content-between pb-4">
        <h1>Available Brands</h1>
        <button class="btn btn-success new-car" data-bs-toggle="modal" data-bs-target="#addModal" data-bs-whatever="@mdo">New Brand</button>
    </div>
    <h1 v-if="error" class="text-info">{{ error }}</h1>
    <CardWidget>
    <div class="col gap-2 mb-5" v-for="brand in brands" :key="brand.brand_id">
        <div class="card">
        <img :src="require('@/assets/uploads/brands/' + brand.image)" class="card-img-top" :alt="brand.name" />
          <div class="card-body">
            <h5 class="card-title">{{ brand.name }}</h5>
            <p class="card-text">{{ brand.description }}</p>
          </div>
          <div class="card-footer d-flex justify-content-between">
            <button class="btn btn-info" data-bs-toggle="modal" data-bs-target="#editModal" @click="showBrandForEdit(brand)">Edit</button>
            <button class="btn btn-danger" @click="deleteBrand(brand.brand_id)">Delete</button>
          </div>
        </div>
      </div>
    </CardWidget>
    <ModalAddWidget>
        <form @submit.prevent="createNewBrand" class="p-4" enctype="multipart/form-data">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">Add New Brand</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                    <div class="mb-3">
                        <label for="brandName" class="col-form-label">Brand Name:</label>
                        <input type="text" class="form-control" placeholder="Brand Name" v-model="name" id="brandName">
                    </div>
                    <div class="mb-3">
                        <label for="description" class="col-form-label">Description:</label>
                        <input type="text" class="form-control" placeholder="Description" v-model="description" id="description">
                    </div>
                    <div class="mb-3">
                        <label for="brandImage" class="col-form-label">Brand Image:</label>
                        <input type="file" class="form-control" placeholder="Brand Image" @change="handleFileChange" id="brandImage">
                    </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                <input type="submit" class="btn btn-primary" value="Create" data-bs-dismiss="modal">
            </div>
        </form>
    </ModalAddWidget>
    <ModalEditWidget>
        <form @submit.prevent="EditBrand(brandForEdit.brand_id)" class="p-4" enctype="multipart/form-data">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">Edit Car</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                    <div class="mb-3">
                        <label for="brand" class="col-form-label">Brand Name:</label>
                        <input type="text" class="form-control" placeholder="Brand" v-model="brandForEdit.name" id="brandName">
                    </div>
                    <div class="mb-3">
                        <label for="description" class="col-form-label">Description:</label>
                        <input type="text" class="form-control" placeholder="Description" v-model="brandForEdit.description" id="description">
                    </div>
                    <div class="mb-3">
                        <label for="brandImage" class="col-form-label">Brand Image:</label>
                        <input type="file" class="form-control" placeholder="Brand Image" @change="handleFileChange" id="brandImage">
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
import ModalAddWidget from '@/components/widgets/ModalAddWidget.vue';
import ModalEditWidget from '@/components/widgets/ModalEditWidget.vue';
import CardWidget from '@/components/widgets/CardWidget.vue';
import axios from 'axios';
import { ref, onMounted, reactive } from 'vue';
export default {
    
    components: { CardWidget, ModalAddWidget, ModalEditWidget },
    setup() {
        const brands = ref([]);
        const error = ref(null);

        // ADD FORM FIELDS :

        const name = ref('');
        const description = ref('');
    
        //
        const image = ref(null);
        //
        
        // EDITED CAR :

        const brandForEdit = reactive({});
        
        onMounted(async ()=> {
            try {
                const response = await axios.get('brands', { withCredentials: true });
                brands.value = await response.data;
            }catch(err) {
                error.value = 'no data available for the moment!'
            }

        });

        const createNewBrand = async () => {
            const formData = new FormData();
            formData.append('name', name.value);
            formData.append('description', description.value);
            formData.append('image', image.value);
            try {
                const response = await axios.post('brands', formData, { withCredentials: true});
                location.reload();
                
            } catch (error) {
                console.log(error);
            }
        }

        const handleFileChange = (event) => {
            return image.value = event.target.files[0];
        }


        const showBrandForEdit = (brand) => {
            console.log(brand)
            return Object.assign(brandForEdit, brand);
        }

        const EditBrand = async (brand_id) => {
            const formData = new FormData();
            if(image.value != null) {
                    formData.append('newImage', image.value);
                    formData.append('image', brandForEdit.image);
            }

            formData.append('name', brandForEdit.name);
            formData.append('description', brandForEdit.description);

            try {
                const response = await axios.put(`brands/${brand_id}`, formData, { withCredentials: true});
                location.reload();
            } catch (error) {
                console.log(error);
            }
        }


        const deleteBrand = async (brand_id) => {
        try {
          const response = await axios.delete(`brands/${brand_id}`, { withCredentials: true });
          console.log(response);
          location.reload();
        } catch (error) {
          console.log(error);
        }
      }

        return { createNewBrand, handleFileChange, showBrandForEdit, deleteBrand, EditBrand, brands, error, name, description, image, brandForEdit }
    }
}
</script>

<style></style>