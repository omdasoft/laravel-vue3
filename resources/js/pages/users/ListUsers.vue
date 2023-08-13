<script setup>
import { ref, onMounted, reactive } from "vue";
import { Form, Field, ErrorMessage } from "vee-validate";
import { object, string, number, date } from "yup";

// import * as yup from "yup";

const users = ref({});
// const form = reactive({
//     name: "",
//     email: "",
//     password: "",
// });

const getUsers = () => {
    axios.get("/api/users").then((response) => {
        users.value = response.data;
        console.log(users);
    });
};

const schema = object({
    name: string().required(),
    email: string().email().required(),
    password: string().required().min(8),
});

const createUser = (values, {resetForm}) => {
    axios.post("/api/users", values).then((respnse) => {
        users.value.unshift(respnse.data);
        $("#createNewUser").modal("hide");
        resetForm();
    });
};
// const createUser = () => {
//     axios.post("/api/users", form).then((respnse) => {
//         users.value.unshift(respnse.data);
//         form.name = "";
//         form.email = "";
//         form.password = "";
//         $("#createNewUser").modal("hide");
//     });
// };

onMounted(() => {
    getUsers();
});
</script>
<template>
    <div>
        <div class="content-header">
            <div class="container-fluid">
                <div class="row mb-2">
                    <div class="col-sm-6">
                        <h1 class="m-0">List Users</h1>
                    </div>
                    <div class="col-sm-6">
                        <ol class="breadcrumb float-sm-right">
                            <li class="breadcrumb-item">
                                <a href="#">Home</a>
                            </li>
                            <li class="breadcrumb-item active">Users</li>
                        </ol>
                    </div>
                </div>
            </div>
        </div>
        <div class="content">
            <div class="container-fluid">
                <button
                    type="button"
                    class="btn btn-primary mb-2"
                    data-toggle="modal"
                    data-target="#createNewUser"
                >
                    Add New User
                </button>
                <div class="card">
                    <div class="card-body">
                        <div class="table-responsive">
                            <table class="table table-bordered table-striped">
                                <thead>
                                    <th>#</th>
                                    <th>Name</th>
                                    <th>Email</th>
                                    <th>Registration Date</th>
                                    <th>Role</th>
                                    <th>Options</th>
                                </thead>
                                <tbody>
                                    <tr
                                        v-for="(user, index) in users"
                                        :key="user.id"
                                    >
                                        <td>{{ ++index }}</td>
                                        <td>{{ user.name }}</td>
                                        <td>{{ user.email }}</td>
                                        <td>{{ user.created_at }}</td>
                                        <td>role</td>
                                        <td>options</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!-- Modal -->
        <div
            class="modal fade"
            id="createNewUser"
            style="display: none"
            aria-hidden="true"
        >
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h4 class="modal-title">Create New User</h4>
                        <button
                            type="button"
                            class="close"
                            data-dismiss="modal"
                            aria-label="Close"
                        >
                            <span aria-hidden="true">×</span>
                        </button>
                    </div>
                    <Form
                        @submit="createUser"
                        :validation-schema="schema"
                        v-slot="{ errors }"
                    >
                        <div class="modal-body">
                            <div class="form-group">
                                <label for="name">Name</label>
                                <Field
                                    name="name"
                                    type="text"
                                    class="form-control"
                                    :class="{ 'is-invalid': errors.name }"
                                    id="name"
                                />
                                <span class="invalid-feedback">{{
                                    errors.name
                                }}</span>
                            </div>
                            <div class="form-group">
                                <label for="email">Email</label>
                                <Field
                                    name="email"
                                    type="text"
                                    class="form-control"
                                    :class="{ 'is-invalid': errors.email }"
                                    id="email"
                                />
                                <span class="invalid-feedback">{{
                                    errors.email
                                }}</span>
                            </div>
                            <div class="form-group">
                                <label for="name">Password</label>
                                <Field
                                    name="password"
                                    type="password"
                                    class="form-control"
                                    :class="{ 'is-invalid': errors.password }"
                                    id="password"
                                />
                                <span class="invalid-feedback">{{
                                    errors.password
                                }}</span>
                            </div>
                            <div class="d-flex justify-content-end">
                                <button
                                    type="button"
                                    class="btn btn-default mr-2"
                                    data-dismiss="modal"
                                >
                                    Cancel
                                </button>
                                <button type="submit" class="btn btn-primary">
                                    Save changes
                                </button>
                            </div>
                        </div>
                    </Form>
                </div>
            </div>
        </div>
    </div>
</template>
