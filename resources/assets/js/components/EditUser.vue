<template>
    <div>
        <h1>Edit User</h1>
        <form @submit.prevent="updateForm"  method="post" enctype="multipart/form-data">
            <div class="form-group">
                <input class="form-control title" type="text" name="name" placeholder="Title" v-model="user.name">
            </div>
            <div class="form-group">
                <input class="form-control title" type="text" name="email" placeholder="Email" v-model="user.email">
            </div>
            <div class="form-group">
                <input class="form-control title" type="password" name="password" placeholder="Password" v-model="user.password">
            </div>
            <div class="form-group">
                <div v-if="!image">
                    <h2>Select an image</h2>
                </div>
                <div v-else>
                    <img :src="image"/>
                    <button @click="removeImage">Remove image</button>
                </div>
                <input name="image" type="file" @change="onFileChange">
            </div>
            <button class="btn btn-primary" type="submit">Edit User</button>
        </form>
    </div>
</template>
<style>
img {
    width: 30%;
    margin: auto;
    display: block;
    margin-bottom: 10px;
}

</style>
<script>

    export default{
        data(){

            return{
                image:'',
                user:{
                    img:''
                },
                baseUrl: '/',
            }
        },
        methods: {
        fetchTask: function(id)
        {
                this.$http.get(this.baseUrl + 'api/users/'+ id).then(response => {
                    this.user =  response.data.users;
                }, (response) => {
                    console.log(response)
                });
        },
        onFileChange(e) {
        var files = e.target.files || e.dataTransfer.files;
        if (!files.length)
        return;
        this.createImage(files[0]);
        },
        createImage(file) {
        var image = new Image();
        var reader = new FileReader();
        var vm = this;
        reader.onload = (e) => {
        vm.image = e.target.result;
        this.user.img = e.target.result;
        };
        reader.readAsDataURL(file);
        },
        removeImage: function (e) {
        this.image = '';
        this.user.img = e.target.result;        
        },
        updateForm :function(){

        this.$http.put(this.baseUrl + 'api/users/' + this.$route.params.id, this.user).then((response) => {
        this.$router.push({path: '/', })
        }, (response) => {
        });
        }
        },
        created:function(){
        this.fetchTask(this.$route.params.id);
        }

    }

</script>
