<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-12">
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title pt-2">users </h3>

                        <div class="card-tools">
                            <button data-toggle="modal" data-target="#adminModalCenter"
                                    class="btn btn-info"
                                    @click="newUser"
                            >
                                <i class="fa fa-plus mr-1"></i>Add User
                            </button>
                        </div>

                    </div>
                    <!-- /.card-header -->
                    <div class="card-body table-responsive p-0">
                        <table class="table table-hover text-nowrap">
                            <thead>
                            <tr>
                                <th>Name</th>
                                <th>Email</th>
                                <th>Date</th>
                                <th>Type</th>
                                <th>Action</th>
                            </tr>
                            </thead>
                            <tbody>
                            <tr v-for="user in users" :key="user.id">
                                <td>{{ user.name }}</td>
                                <td>{{ user.email }}</td>
                                <td>{{ user.created_at | date }}</td>
                                <td>{{ user.type | capital }}</td>
                                <td>
                                    <button class="fa fa-edit text-info" @click="editUser(user)"></button>
                                    /
                                    <button @click="deleteUser(user.id)"><i class="fa fa-trash text-danger"></i></button>
                                </td>
                            </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
        <!-- Button trigger modal -->


        <!-- Modal -->
        <div class="modal fade" id="adminModalCenter" tabindex="-1" role="dialog" aria-labelledby="adminModalCenterTitle" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="addNewModalLongTitle" v-show="!editMode">Add User</h5>
                        <h5 class="modal-title" id="addNewModalLongTitle" v-show="editMode">Update User</h5>                        <button type="button" class="close" @click="fetchUser()" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <form @submit.prevent="editMode? editUserinfo() : addUser() " @keydown="form.onKeydown($event)">

                    <div class="modal-body">
                            <!--<input v-model="form.username" type="text" name="username" placeholder="Username">-->
                            <!--<div v-if="form.errors.has('username')" v-html="form.errors.get('username')" />-->
<!---->
                            <!--&lt;!&ndash;&ndash;&gt;<input v-model="form.password"  name="password" placeholder="Password"-->
                                   <!--type="checkbox">-->
                            <!--<div v-if="form.errors.has('password')" v-html="form.errors.get('password')" />-->



                            <!--<form @submit.prevent="login" @keydown="form.onKeydown($event)">-->
                                <!--<input v-model="form.username" type="text" name="username" placeholder="Username">-->
                                <!--&lt;!&ndash;<div v-if="form.errors.has('username')" v-html="form.errors.get('username')" />&ndash;&gt;-->

                                <!--<input v-model="form.password" type="password" name="password" placeholder="Password">-->
                                <!--&lt;!&ndash;<div v-if="form.errors.has('password')" v-html="form.errors.get('password')" />&ndash;&gt;-->

                                <!--<button type="submit" :disabled="form.busy">-->
                                    <!--Log In-->
                                <!--</button>-->
                            <!--</form>-->


                        <!--<form @submit.prevent="login" @keydown="form.onKeydown($event)">-->
                            <!--<div class="form-group">-->
                                <label>Name</label>
                                <!--<input v-model="form.name" type="text" name="name" placeholder="Username">-->
                                <!--<div v-if="form.errors.has('name')" v-html="form.errors.get('name')" />-->

                                <!--&lt;!&ndash;<input v-model="form.password" type="password" name="password" placeholder="Password">&ndash;&gt;-->
                                <!--&lt;!&ndash;<div v-if="form.errors.has('password')" v-html="form.errors.get('password')" />&ndash;&gt;-->

                                <input
                                        v-model="form.name"
                                        type="text"
                                        name="name"
                                        class="form-control"
                                        :class="{ 'is-invalid': form.errors.has('name') }"
                                />
                                        <has-error :form="form" field="name"></has-error>
                                <label>Email</label>
                                <input
                                        v-model="form.email"
                                        type="text"
                                        name="email"
                                        class="form-control"
                                        :class="{ 'is-invalid': form.errors.has('email') }"
                                />
                                <has-error :form="form" field="email"></has-error>

                                <label>Password</label>
                                <input
                                        v-model="form.password"
                                        type="text"
                                        name="password"
                                        class="form-control"
                                        :class="{ 'is-invalid': form.errors.has('password') }"
                                />
                                <has-error :form="form" field="password"></has-error>

                                <label>Type</label>
                                <input
                                        v-model="form.type"
                                        type="text"
                                        name="type"
                                        class="form-control"
                                        :class="{ 'is-invalid': form.errors.has('type') }"
                                />
                                <has-error :form="form" field="type"></has-error>

                                <label>Bio</label>
                                <textarea
                                        v-model="form.bio"
                                        type="text"
                                        name="bio"
                                        class="form-control"
                                        :class="{ 'is-invalid': form.errors.has('bio') }"
                                />
                                <has-error :form="form" field="bio"></has-error>
                              <!--</div>-->
                        <!--</form>-->
                    </div>
                    <div class="modal-footer">

                        <!--<button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>-->
                        <button type="submit" :disabled="form.busy" v-show="!editMode"  class="btn btn-primary">Save changes</button>
                        <button type="submit" :disabled="form.busy" v-show="editMode" class="btn btn-success">update</button>
                    </div></form>
                </div>
            </div>
        </div>
    </div>
</template>
<!--<script>-->
    <!--import Form from 'vform'-->

    <!--export default {-->
        <!--data: () => ({-->
            <!--form: new Form({-->
                <!--username: '',-->
                <!--password: ''-->
            <!--})-->
        <!--}),-->

        <!--methods: {-->
            <!--async login () {-->
                <!--const response = await this.form.post('/api/user')-->
                <!--// ...-->
            <!--}-->
        <!--}-->
    <!--}-->
<!--</script>-->
<script>
    // console.log(users);
   import Form from 'vform'
    $('#myModal').on('hidden.bs.modal', function () {
        // do something…
    });
    export default {
        data() {
            return {
                users: {},
                editMode: false,
                edittest: false,
                // Create a new form instance
                form: new Form({
                    id:"",
                    name: "",
                    email: "",
                    password: "",
                    type: "",
                    bio: "",
                    photo: "",
                }),
            };
        },
        created(){
            this.fetchUser()
        //     axios.get('api/user')
        //         .then(({data}) => (this.users = data.data))

        },
        methods: {
            editUserinfo(){
                this.edittest=true;
              this.form.put('api/user/'+this.form.id).then(()=>
                  Toast.fire({
                      icon: 'success',
                      title: 'User Updateed successfully'
                  })
              );
                $('#adminModalCenter').modal('hide');
                this.fetchUser();

                // this.edittest='api/user/'+this.form.id
            },
            newUser() {

                $('#adminModalCenter').modal('show')

            },
            editUser(user) {
                this.editMode = true;
                this.form .reset();
                $('#adminModalCenter').modal('show');

                this.form.fill(user);
            },

            fetchUser() {
                this.editMode = false;
                $('#adminModalCenter').modal('hide');
                axios.get('api/user')
                    .then(({data}) => (this.users = data.data))
            },

            addUser() {
                this.$Progress.start();
                  this.form.post('/api/user')
                    .then(() => {
                    this.form .reset();
                    $('#adminModalCenter').modal('hide');
                    this.fetchUser();
                    // Fire.$emit('afterCreate')
                    this.$Progress.finish();
                    Toast.fire({
                        icon: 'success',
                        title: 'User Created successfully'
                    })
                })
            },
            deleteUser(id)
            {
                this.form.delete('api/user/' + id)
                    .then(() => {
                        Swal.fire({
                            title: 'Are you sure?',
                            text: "You won't be able to revert this!",
                            icon: 'warning',
                            showCancelButton: true,
                            confirmButtonColor: '#3085d6',
                            cancelButtonColor: '#d33',
                            confirmButtonText: 'Yes, delete it!'
                        }).then((result) => {
                            if (result.isConfirmed) {
                                Swal.fire(
                                    'Deleted!',
                                    'Your file has been deleted.',
                                    'success'
                                )
                                this.fetchUser();
                            }

                        })
                            .catch(() => {
                                if (result.isConfirmed) {
                                    Swal.fire(
                                        'User Not Deleted!',
                                        'Your file has been deleted.',
                                        'danger'
                                    )
                                }
                            })
                    })
            },



        },

    }
// </script>
<!--<script>-->
    <!--export default {-->
        <!--data() {-->
            <!--return {-->
                <!--users: {},-->
                <!--editMode: false,-->
                <!--// Create a new form instance-->
                <!--form: new Form({-->
                    <!--name: "",-->
                    <!--email: "",-->
                    <!--password: "",-->
                    <!--type: "",-->
                    <!--bio: "",-->
                    <!--photo: "",-->
                <!--}),-->
            <!--};-->
        <!--},-->
        <!--methods: {-->

            <!--createUser() {-->
                <!--this.editMode = false;-->
                <!--axios.get('api/user')-->
                    <!--.then(({data}) => (this.users = data.data))-->
            <!--},-->
            <!--newUser() {-->

                <!--$('#addNewModalCenter').modal('show')-->

            <!--},-->

        <!--},-->
        <!--created() {-->
            <!--this.createUser()-->
          <!---->
        <!--},-->
        <!--mounted() {-->
            <!--console.log("Component mounted.");-->
        <!--},-->
    <!--};-->
<!--</script>-->
<!--<script>-->
    <!--import Form from 'vform'-->

    <!--export default {-->
        <!--data: () => ({-->
            <!--form: new Form({-->
                <!--username: '',-->
                <!--password: ''-->
            <!--})-->
        <!--}),-->

        <!--methods: {-->
            <!--async login () {-->
                <!--const response = await this.form.post('/api/user')-->
                <!--// ...-->
            <!--}-->
        <!--}-->
    <!--}-->
<!--</script>-->