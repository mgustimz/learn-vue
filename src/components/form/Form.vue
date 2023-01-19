<template>
    <section>
        <div>
            <div class="fetch-profile">
                <button @click="fetchList" class="btn-users">Fetch Form</button>
                <h4 v-if="loading">Loading...</h4>
            </div>
            <div>
                <table class="user-table">
                    <thead class="thead-bg">
                        <tr>
                            <th scope="col">#</th>
                            <th scope="col">Name</th>
                            <th scope="col">Username</th>
                            <th scope="col">Email</th>
                        </tr>
                    </thead>
                    <tbody v-for="(user, id) in list" :key="id">
                        <tr>
                            <th scope="row">{{ user.id }}</th>
                            <td>{{ user.fullName }}</td>
                            <td>{{ user.gender }}</td>
                            <td>{{ user.email }}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </section>

    <section>
        <form>
            <input type="text" v-model="form.registrationPeriod">
        <br>
            <input type="text" v-model="form.fullName">
            <br>
            <input type="date" v-model="form.birthDate">
            <br>
            <input type="text" v-model="form.placeOfBirth">
            <br>
            <input type="text" v-model="form.gender">
            <br>
            <input type="email" v-model="form.email">
            <br>
            <button type="submit" @click="saveForm(form)">add</button>
        </form>
    </section>
</template>

<script>
import axios from 'axios';
export default {
    name: "GetPage",

    data() {
        return {
            list: [],
            loading: false,
            form: {
                fullName: ''
            }
        };
    },
    methods: {
        fetchList() {
            this.loading = true;
            this.list = [];
            axios
                .get("http://localhost:8080/form")
                .then((response) => {
                    console.log(response)
                    const data = response.data; // [{}, {}]
                    this.list = data.list;
                    this.loading = false;
                });
        },
        saveForm(form) {
            axios
                .post("http://localhost:8081/form", form)
                .then((response) => {
                    console.log(response)
                    const data = response.data;
                    this.list.push(data);
                    this.fullName = "";
                });
        }
    },
};
</script>