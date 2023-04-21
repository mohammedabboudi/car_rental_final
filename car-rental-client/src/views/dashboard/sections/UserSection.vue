<template>
    <table class="table">
        <thead class="table-dark">
            <tr>
            <th scope="col">#</th>
            <th scope="col">Prfile</th>
            <th scope="col">First Name</th>
            <th scope="col">Last name</th>
            <th scope="col">Email</th>
            <th scope="col">Action</th>
            
            </tr>
        </thead>
        <tbody v-for="user, index in users">
            <tr>
            <th scope="row">{{  index+1 }}</th>
            <td> hhhh </td>
            <td>{{ user.first_name }}</td>
            <td>{{ user.last_name }}</td>
            <td>{{ user.email }}</td>
            <td><button class="btn btn-danger" @click="removeUser(user.user_id)">Remove</button></td>
            </tr>
        </tbody>
</table>
</template>

<script>
import axios from 'axios';
import { ref, onMounted } from 'vue';

export default {
    setup() {

        const users = ref([]);

        onMounted(async () => {
            const response = await axios.get('users', { withCredentials: true })
            users.value = response.data;

        })


        const removeUser = async (user_id) => {
            const response = await axios.delete(`users/${user_id}`, { withCredentials: true});
            location.reload();
        }

        return { users, removeUser }
    }
}
</script>

<style></style>