<template>
    <section>
        <div>
            <div class="fetch-form">
                <button @click="fetchList" class="btn-users">Fetch Form</button>
                <h4 v-if="loading">Loading...</h4>
            </div>
            <div>
                <table class="table table-bordered table-dark">
                    <thead>
                        <tr>
                            <th scope="col">#</th>
                            <th scope="col">Gelombang</th>
                            <th scope="col">Full Name</th>
                            <th scope="col">Place of Birth</th>
                            <th scope="col">Birth Date</th>
                            <th scope="col">Gender</th>
                            <th scope="col">Email</th>
                        </tr>
                    </thead>
                    <tbody v-for="(user, id) in list" :key="id">
                        <tr>
                            <th scope="row">{{ user.id }}</th>
                            <td>{{ user.registrationPeriod }}</td>
                            <td>{{ user.fullName }}</td>
                            <td>{{ user.placeOfBirth }}</td>
                            <td>{{ user.birthDate }}</td>
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
            <div class="form-group">
                <label for="exampleInputEmail">Registration Period</label>
                <input type="text" class="form-control" v-model="form.registrationPeriod"
                    placeholder="Registration Period">
                <small id="emailHelp" class="form-text text-muted">We'll never share your email with anyone
                    else.</small>
            </div>
            <div class="form-group">
                <label>Full Name</label>
                <input type="text" class="form-control" v-model="form.fullName" placeholder="Full Name">
            </div>
            <div class="form-group">
                <label>Place of Birth</label>
                <input type="text" class="form-control" v-model="form.placeOfBirth" placeholder="Place of Birth">
            </div>
            <div class="form-group">
                <label>Birth Date</label>
                <input type="date" class="form-control" v-model="form.birthDate" placeholder="Birth Date">
            </div>
            <div class="form-group">
                <label>Gender</label>
                <input type="text" class="form-control" v-model="form.gender" placeholder="Gender">
            </div>
            <div class="form-group">
                <label>Email</label>
                <input type="email" class="form-control" v-model="form.email" placeholder="Email">
            </div>
            <button type="submit" class="btn btn-primary" @click="saveForm(form)">Submit</button>
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

<style>
.form-group {
  max-width: 500px;
  margin: auto;
}
</style>