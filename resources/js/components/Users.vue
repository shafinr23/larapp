
<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">

                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">User Table</h3>

                        <div class="card-tools">
                            <button class="btn btn-success"  data-toggle="modal" data-target="#addnew">Add New User <i class="fas fa-user-alt fa-fw"></i></button>
                        </div>
                    </div>
                    <!-- /.card-header -->
                    <div class="card-body table-responsive p-0">
                        <table class="table table-hover">
                            <tbody><tr>
                                <th>ID</th>
                                <th>name</th>
                                <th>Email</th>
                                <th>Type</th>
                                <th>Created At</th>
                                <th>Modify</th>
                            </tr>
                            <tr v-for="user in users" :key="user.id" >
                                <td>{{ user.id}}</td>
                                <td>{{ user.name | upText }}</td>
                                <td>{{ user.email }}</td>
                                <td>{{ user.type | upText }}</td>
                                <td>{{ user.created_at | mydate }}</td>

                                <td><a href="#">
                                    <i class="fas fa-edit cyan" ></i>
                                </a>
                                    /
                                    <a href="#">
                                        <i class="fas fa-trash red" ></i>
                                    </a>

                                </td>
                            </tr>

                            </tbody></table>
                    </div>
                    <!-- /.card-body -->
                </div>
                <!-- /.card -->

            </div>
        </div>
        <!-- Modal -->
        <div class="modal fade" id="addnew" tabindex="-1" role="dialog" aria-labelledby="addnewLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="addnewLabel">Add New User </h5>
                        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <form @submit.prevent="createUser">
                    <div class="modal-body">
                       <div class="form-group">
                           <input v-model="form.name" type="text" placeholder="Name" name="name"
                                  class="form-control" :class="{ 'is-invalid': form.errors.has('name') }">
                           <has-error :form="form" field="name"></has-error>
                       </div>
                        <div class="form-group">
                            <input v-model="form.email" type="text" placeholder="Email" name="email"
                                   class="form-control" :class="{ 'is-invalid': form.errors.has('email') }">
                            <has-error :form="form" field="email"></has-error>
                        </div>
                        <div class="form-group">
                            <textarea v-model="form.bio" type="text" placeholder="Bio (optional)" name="bio"
                                      class="form-control" :class="{ 'is-invalid': form.errors.has('bio') }"></textarea>
                            <has-error :form="form" field="bio"></has-error>
                        </div>
                        <div class="form-group">
                            <select name="type" v-model="form.type" id="type"  class="form-control" :class="{ 'is-invalid': form.errors.has('type') }" >
                                <option value="">Select User Role </option>
                                <option value="admin">Admin </option>
                                <option value="user">User </option>
                                <option value="author">Author </option>

                            </select>
                            <has-error :form="form" field="type"></has-error>
                        </div>
                        <div class="form-group">
                            <input v-model="form.password" type="password" placeholder="password" name="password"
                                      class="form-control" :class="{ 'is-invalid': form.errors.has('password') }">
                            <has-error :form="form" field="password"></has-error>
                        </div>

                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                        <button type="submit" class="btn btn-primary">Creat </button>
                    </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Form from 'vform';

    export default {
        data(){
          return {
              users:{},
              form: new Form({
                  name:'',
                  email:'',
                  password:'',
                  type:'',
                  bio:'',
                  photo:''
              })
            }
        },
        methods:{
            loadUsers(){
                axios.get('api/user').then(({ data })=>(this.users = data.data  ));
            },
            createUser(){
                this.form.post('api/user');
            }
        },
        created() {
           this.loadUsers();
        }
    }
</script>
