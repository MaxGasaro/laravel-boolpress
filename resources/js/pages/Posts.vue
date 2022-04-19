<template>
    <div class="container">
        <h1>Elenco dei post</h1>
        <div class="row">
            <div class="col-6" v-for="post in posts" :key="post.id">
                <div class="card">
                    <div class="card-header font-weight-bold">{{post.title}}</div>

                    <div class="card-body">
                        <h5 class="card-title">{{post.title}}</h5>
                        <h3 class="card-text">{{post.category?post.category.name:'&nbsp;'}}</h3>
                        <p class="card-text">{{post.content.substring(0,60)}}</p>
                        <router-link :to="{name: 'single-post', params: {slug: post.slug}}" class="btn btn-primary">Vedi Articolo Completo</router-link>
                    </div>
                </div>
            </div>
        </div>
        <div class="my-4">
            <nav aria-label="Page navigation example">
                <ul class="pagination">
                    <li class="page-item" :class="(currentPage == 1)?'disabled':''"><a class="page-link" @click="getPosts(currentPage - 1)" href="#">Previous</a></li>
                    <li class="page-item" :class="(currentPage == lastPage)?'disabled':''"><a class="page-link" @click="getPosts(currentPage + 1)" href="#">Next</a></li>
                </ul>
            </nav>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Posts',
        data() {
            return {
                posts: [],
                currentPage: 1,
                lastPage: null
            }
        },
        methods: {
            getPosts(apiPage) {
                axios.get('/api/posts', {
                    'params': {
                        'page': apiPage
                    }
                }).then((response) => {
                    this.currentPage = response.data.results.current_page;
                    this.posts = response.data.results.data;
                    this.lastPage = response.data.results.last_page;
                }).catch((err) => {
                //
                });
            }
        },
        created() {
            this.getPosts(1)
        }
    }
</script>