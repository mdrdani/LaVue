<style>
.widget-user-header {
    background-position: center center;
    background-size: cover;
}
</style>

<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12 mt-4">
                <div class="card card-widget widget-user">
                    <!-- Add the bg color to the header using any of the bg-* classes -->
                    <div
                        class="widget-user-header text-white"
                        style="background-image:url('./img/user-cover.jpg'); height:300px"
                    >
                        <h3 class="widget-user-username text-right">
                            {{ this.form.name }}
                        </h3>
                        <h5 class="widget-user-desc text-right">
                            {{ this.form.type }}
                        </h5>
                    </div>
                    <div class="widget-user-image mx-auto d-block">
                        <img
                            class="img-circle w-50"
                            :src="getProfilePhoto()"
                            alt="User Avatar"
                        />
                    </div>
                </div>
            </div>
            <div class="col-md-12 mt-4">
                <div class="card">
                    <div class="card-header p-2">
                        <ul class="nav nav-pills">
                            <li class="nav-item">
                                <a
                                    class="nav-link active"
                                    href="#activity"
                                    data-toggle="tab"
                                    >Activity</a
                                >
                            </li>
                            <li class="nav-item">
                                <a
                                    class="nav-link"
                                    href="#settings"
                                    data-toggle="tab"
                                    >Settings</a
                                >
                            </li>
                        </ul>
                    </div>
                    <!-- /.card-header -->
                    <div class="card-body">
                        <div class="tab-content">
                            <div class="tab-pane active" id="activity">
                                <h2>This Is Activity</h2>
                            </div>
                            <!-- /.tab-pane -->

                            <div class="tab-pane" id="settings">
                                <form class="form-horizontal">
                                    <div class="form-group row">
                                        <label
                                            for="inputName"
                                            class="col-sm-2 col-form-label"
                                            >Name</label
                                        >
                                        <div class="col-sm-10">
                                            <input
                                                type="text"
                                                v-model="form.name"
                                                class="form-control"
                                                id="inputName"
                                                placeholder="Name"
                                                :class="{
                                                    'is-invalid': form.errors.has(
                                                        'name'
                                                    )
                                                }"
                                            />
                                            <has-error
                                                :form="form"
                                                field="name"
                                            ></has-error>
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label
                                            for="inputEmail"
                                            class="col-sm-2 col-form-label"
                                            >Email</label
                                        >
                                        <div class="col-sm-10">
                                            <input
                                                type="email"
                                                v-model="form.email"
                                                class="form-control"
                                                id="inputEmail"
                                                placeholder="Email"
                                                :class="{
                                                    'is-invalid': form.errors.has(
                                                        'email'
                                                    )
                                                }"
                                            />
                                            <has-error
                                                :form="form"
                                                field="email"
                                            ></has-error>
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label
                                            for="inputExperience"
                                            class="col-sm-2 col-form-label"
                                            >Experience</label
                                        >
                                        <div class="col-sm-10">
                                            <textarea
                                                v-model="form.bio"
                                                class="form-control"
                                                id="inputExperience"
                                                placeholder="Experience"
                                            ></textarea>
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label
                                            for="inputfotoprofile"
                                            class="col-sm-2 col-form-label"
                                        >
                                            Profile Photo
                                        </label>
                                        <div class="col-sm-10">
                                            <div class="input-group">
                                                <div class="custom-file">
                                                    <input
                                                        type="file"
                                                        @change="updateProfile"
                                                        class="custom-file-input"
                                                        id="exampleInputFile"
                                                    />
                                                    <label
                                                        class="custom-file-label"
                                                        for="exampleInputFile"
                                                        >Choose file</label
                                                    >
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label
                                            for="inputSkills"
                                            class="col-sm-2 col-form-label"
                                            >Passport (leave empty if not
                                            changing)</label
                                        >
                                        <div class="col-sm-10">
                                            <input
                                                type="password"
                                                v-model="form.password"
                                                class="form-control"
                                                id="inputSkills"
                                                placeholder="Password"
                                                :class="{
                                                    'is-invalid': form.errors.has(
                                                        'password'
                                                    )
                                                }"
                                            />
                                            <has-error
                                                :form="form"
                                                field="password"
                                            ></has-error>
                                        </div>
                                    </div>

                                    <div class="form-group row">
                                        <div class="offset-sm-2 col-sm-10">
                                            <button
                                                @click.prevent="updateInfo"
                                                type="submit"
                                                class="btn btn-success"
                                            >
                                                Update
                                            </button>
                                        </div>
                                    </div>
                                </form>
                            </div>
                            <!-- /.tab-pane -->
                        </div>
                        <!-- /.tab-content -->
                    </div>
                    <!-- /.card-body -->
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            form: new Form({
                id: "",
                name: "",
                email: "",
                password: "",
                type: "",
                bio: "",
                photo: ""
            })
        };
    },
    mounted() {},
    methods: {
        getProfilePhoto() {
            let photo =
                this.form.photo.length > 200
                    ? this.form.photo
                    : "img/profile/" + this.form.photo;
            return photo;
        },
        updateProfile(e) {
            let file = e.target.files[0];
            // console.log(file);
            let reader = new FileReader();
            if (file["size"] < 2111775) {
                reader.onloadend = file => {
                    this.form.photo = reader.result;
                };
                reader.readAsDataURL(file);
            } else {
                Swal.fire({
                    icon: "error",
                    title: "Oops...",
                    text: "You Are uploading a large file"
                });
            }
        },
        updateInfo() {
            this.$Progress.start();
            this.form
                .put("api/profile")
                .then(() => {
                    this.$Progress.finish();
                    Swal.fire(
                        "Good job!",
                        "Profile Has been Update",
                        "success"
                    );
                })
                .catch(() => {
                    this.$Progress.fail();
                });
        }
    },
    created() {
        axios.get("api/profile").then(({ data }) => this.form.fill(data));
    }
};
</script>
