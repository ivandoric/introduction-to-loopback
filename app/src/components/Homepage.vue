<template>
    <div class="container my-5">
        <div class="row justify-content-center">
            <div class="col-6">
                <div class="card mb-5">
                    <div class="card-body">
                        <div class="form-gorup mb-3">
                            <input type="email" name="" class="form-control" placeholder="Enter email" v-model="email">
                        </div>
                        <div class="form-gorup mb-3">
                            <input type="password" name="" class="form-control" placeholder="Enter password" v-model="password">
                        </div>
                        <button class="btn btn-primary" v-on:click="login">Login</button>
                    </div>
                </div>

                <div class="card mb-5">
                    <div class="card-body">
                        <div class="form-gorup mb-3">
                            <input type="text" name="" id="" class="form-control" placeholder="Enter the title" v-model="title">
                        </div>
                        <div class="form-gorup mb-3">
                            <textarea name="" id="" cols="30" rows="10" class="form-control" placeholder="Enter your content" v-model="content"></textarea>
                        </div>
                        <button class="btn btn-primary" v-on:click="addPost">Add post</button>
                    </div>
                </div>


                <h2>Post list</h2>

                <div class="card mb-3" v-for="post in posts">
                    <div class="card-body">
                        <h3>{{post.title}}</h3>
                        <p v-text="post.content"></p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
    import axios from 'axios'

    export default {
        data() {
            return {
                message: "Hello World",
                posts: [],
                title: "",
                content: "",
                email: "",
                password: "",
                token: localStorage.getItem('accToken')
            }
        },

        mounted() {
            this.getPosts()
        },

        methods: {
            addPost() {
                const data = {
                    title: this.title,
                    content: this.content
                }

                axios.post('http://localhost:3000/api/Posts?access_token=' + this.token, data).then(response => {
                    console.log(response)
                    this.getPosts()
                    this.clearInput()
                })
            },

            getPosts() {
                axios.get('http://localhost:3000/api/Posts').then(response => {
                    this.posts = response.data
                    this.posts.reverse()
                })
            },

            clearInput() {
                this.title = ""
                this.content = ""
            },

            login() {
                const credentials = {
                    email: this.email,
                    password: this.password
                }

                axios.post('http://localhost:3000/api/Users/login', credentials).then(response => {
                    console.log(response)
                    this.token = response.data.id
                    localStorage.setItem('accToken', this.token)
                })
            }
        }
    }
</script>