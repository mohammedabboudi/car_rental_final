<template>
    <div class="d-flex justify-content-between pb-4">
        <h1>Our Employees</h1>
        <button class="btn btn-success new-employee" data-bs-toggle="modal" data-bs-target="#addModal" data-bs-whatever="@mdo">New Employee</button>
    </div>
    <table class="table">
        <thead class="table-dark">
            <tr>
            <th scope="col">#</th>
            <th scope="col">First Name</th>
            <th scope="col">Last Name</th>
            <th scope="col">Email</th>
            <th scope="col">Number Phone</th>
            <th scope="col">Address</th>
            <th scope="col">Actions</th>
            </tr>
        </thead>
        <tbody v-for="employee, index in employees">
            <tr>
            <th scope="row">{{ index+1 }}</th>
            <td>{{ employee.first_name }}</td>
            <td>{{ employee.last_name }}</td>
            <td>{{ employee.email_name }}</td>
            <td>{{ employee.phone_number }}</td>
            <td>{{ employee.address }}</td>
            <td>
                <div class="d-flex"> 
                    <button class="btn btn-info"  data-bs-toggle="modal" data-bs-target="#editModal" @click="showEmployeeForEdit(employee)">Edit</button> 
                    <button class="btn btn-danger ms-2" @click="deleteEmployee(employee.employee_id)">Delete</button> 
                </div>
            </td>
            </tr>
        </tbody>
</table>
<ModalAddWidget>
        <form @submit.prevent="createNewEmployee" class="p-4" enctype="multipart/form-data">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">Add New Employee</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                    <div class="mb-3">
                        <!-- <label for="employeeFirstName" class="col-form-label">First Name:</label> -->
                        <input type="text" class="form-control" placeholder="First Name" v-model="first_name" id="employeeFirstName">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="employeeLastName" class="col-form-label">Last Name:</label> -->
                        <input type="text" class="form-control" placeholder="Last Name" v-model="last_name" id="employeeLastName">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="email" class="col-form-label">Email:</label> -->
                        <input type="email" class="form-control" placeholder="Email" v-model="email" id="email">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="phone" class="col-form-label">Phone Number:</label> -->
                        <input type="text" class="form-control" placeholder="Phone Number" v-model="phone_number" id="phone">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="image" class="col-form-label">Brand Image:</label> -->
                        <input type="file" class="form-control" placeholder="Employee Image" @change="handleFileChange" id="image">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="address" class="col-form-label">Address:</label> -->
                        <input type="text" class="form-control" placeholder="Address" v-model="address" id="address">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="city" class="col-form-label">City:</label> -->
                        <input type="text" class="form-control" placeholder="City" v-model="city" id="city">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="state" class="col-form-label">State:</label> -->
                        <input type="text" class="form-control" placeholder="State" v-model="state" id="state">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="zipcode" class="col-form-label">Zipcode:</label> -->
                        <input type="text" class="form-control" placeholder="Zipcode" v-model="zip_code" id="zipcode">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="birthday" class="col-form-label">Birthday:</label> -->
                        <input type="date" class="form-control" placeholder="Birthday" v-model="date_of_birth" id="birthday">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="hiredate" class="col-form-label">Hire date:</label> -->
                        <input type="date" class="form-control" placeholder="Hiredate" v-model="date_of_hire" id="hiredate">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="jobtitle" class="col-form-label">Job title:</label> -->
                        <input type="text" class="form-control" placeholder="Job title" v-model="job_title" id="jobtitle">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="salary" class="col-form-label">Salary:</label> -->
                        <input type="number" class="form-control" placeholder="Salary" v-model="salary" id="salary">
                    </div>
                    
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                <input type="submit" class="btn btn-primary" value="Create" data-bs-dismiss="modal">
            </div>
        </form>
    </ModalAddWidget>
    <ModalEditWidget>
        <form @submit.prevent="editEmployee(employeeForEdit.employee_id)" class="p-4" enctype="multipart/form-data">
            <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">Edit Employee Details</h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                    <div class="mb-3">
                        <!-- <label for="employeeFirstName" class="col-form-label">First Name:</label> -->
                        <input type="text" class="form-control" placeholder="First Name" v-model="employeeForEdit.first_name" id="employeeFirstName">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="employeeLastName" class="col-form-label">Last Name:</label> -->
                        <input type="text" class="form-control" placeholder="Last Name" v-model="employeeForEdit.last_name" id="employeeLastName">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="email" class="col-form-label">Email:</label> -->
                        <input type="email" class="form-control" placeholder="Email" v-model="employeeForEdit.email" id="email">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="phone" class="col-form-label">Phone Number:</label> -->
                        <input type="text" class="form-control" placeholder="Phone Number" v-model="employeeForEdit.phone_number" id="phone">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="photo" class="col-form-label">Brand Image:</label> -->
                        <input type="file" class="form-control" placeholder="Employee Image" @change="handleFileChange" id="image">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="address" class="col-form-label">Address:</label> -->
                        <input type="text" class="form-control" placeholder="Address" v-model="employeeForEdit.address" id="address">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="city" class="col-form-label">City:</label> -->
                        <input type="text" class="form-control" placeholder="City" v-model="employeeForEdit.city" id="city">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="state" class="col-form-label">State:</label> -->
                        <input type="text" class="form-control" placeholder="State" v-model="employeeForEdit.state" id="state">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="zipcode" class="col-form-label">Zipcode:</label> -->
                        <input type="text" class="form-control" placeholder="Zipcode" v-model="employeeForEdit.zip_code" id="zipcode">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="birthday" class="col-form-label">Birthday:</label> -->
                        <input type="date" class="form-control" placeholder="Birthday" v-model="employeeForEdit.date_of_birth" id="birthday">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="hiredate" class="col-form-label">Hire date:</label> -->
                        <input type="date" class="form-control" placeholder="Hiredate" v-model="employeeForEdit.date_of_hire" id="hiredate">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="jobtitle" class="col-form-label">Job title:</label> -->
                        <input type="text" class="form-control" placeholder="Job title" v-model="employeeForEdit.job_title" id="jobtitle">
                    </div>
                    <div class="mb-3">
                        <!-- <label for="salary" class="col-form-label">Salary:</label> -->
                        <input type="number" class="form-control" placeholder="Salary" v-model="employeeForEdit.salary" id="salary">
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
        const employees = ref([]);
        const error = ref(null);

        // ADD FORM FIELDS :

        const first_name = ref('');
        const last_name = ref('');
        const email = ref('');
        const phone_number = ref('');
        const address = ref('');
        const city = ref('');
        const state = ref('');
        const zip_code = ref('');
        const date_of_birth = ref('');
        const date_of_hire = ref('');
        const job_title = ref('');
        const salary = ref('');
    
        //
        const image = ref(null);
        //
        
        // EDITED CAR :

        const employeeForEdit = reactive({});
        
        onMounted(async ()=> {
            try {
                const response = await axios.get('employees', { withCredentials: true });
                employees.value = response.data;
            }catch(err) {
                error.value = 'no data available for the moment!'
            }

        });

        const createNewEmployee = async () => {
            const formData = new FormData();
            formData.append('first_name', first_name.value);
            formData.append('last_name', last_name.value);
            formData.append('email', email.value);
            formData.append('phone_number', phone_number.value);
            formData.append('image', image.value);
            formData.append('address', address.value);
            formData.append('city', city.value);
            formData.append('state', state.value);
            formData.append('zip_code', zip_code.value);
            formData.append('date_of_birth', date_of_birth.value);
            formData.append('date_of_hire', date_of_hire.value);
            formData.append('job_title', job_title.value);
            formData.append('salary', salary.value);

            try {
                const response = await axios.post('employees', formData, { withCredentials: true});
                location.reload();
                
            } catch (error) {
                console.log(error);
            }
        }

        const handleFileChange = (event) => {
            return image.value = event.target.files[0];
        }


        const showEmployeeForEdit = (employee) => {
            return Object.assign(employeeForEdit, employee);
        }

        const editEmployee = async (employee_id) => {

            const formData = new FormData();

            if(image.value != null) {
                    formData.append('newImage', image.value);
                    formData.append('image', employeeForEdit.image);
            }

            formData.append('first_name', employeeForEdit.first_name);
            formData.append('last_name', employeeForEdit.last_name);
            formData.append('email', employeeForEdit.email);
            formData.append('phone_number', employeeForEdit.phone_number);
            formData.append('address', employeeForEdit.address);
            formData.append('city', employeeForEdit.city);
            formData.append('state', employeeForEdit.state);
            formData.append('zip_code', employeeForEdit.zip_code);
            formData.append('date_of_birth', employeeForEdit.date_of_birth);
            formData.append('date_of_hire', employeeForEdit.date_of_hire);
            formData.append('job_title', employeeForEdit.job_title);
            formData.append('salary', employeeForEdit.salary);

            try {
                const response = await axios.put(`employees/${employee_id}`, formData, { withCredentials: true});
                location.reload();
            } catch (error) {
                console.log(error);
            }
        }


        const deleteEmployee = async (employee_id) => {

            try {
            const response = await axios.delete(`employees/${employee_id}`, { withCredentials: true });
            console.log(response);
            location.reload();
            } catch (error) {
            console.log(error);
            }

            }

        return { 
            createNewEmployee, handleFileChange, showEmployeeForEdit, deleteEmployee, 
            editEmployee, employees, error, first_name, last_name, email, phone_number, address, 
            city, state, zip_code, date_of_birth, date_of_hire, job_title, salary, employeeForEdit,
        }
    }
}
</script>

<style></style>