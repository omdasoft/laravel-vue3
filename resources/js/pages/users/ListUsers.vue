<script setup>
    import { ref, onMounted } from 'vue';

    const users = ref({});
    const getUsers = () => {
        axios.get('/api/users')
        .then((response) => {
            users.value = response.data;
            console.log(users);
        })
    }

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
                                    <tr v-for="(user, index) in users" :key="user.id">
                                        <td>{{++index}}</td>
                                        <td>{{ user.name }}</td>
                                        <td>{{ user.email}}</td>
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
    </div>
</template>
