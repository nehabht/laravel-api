<template>

    <div>

        <h1 class="text-center mt-4"> Work In Progress</h1>

        <section class="posts">
            <div class="container">
                <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3">
                    <div class="col g-4" v-for="post in postsResponse.data" :key="post.id">
                        <div class="product card h-100">
                            <img class="img-fluid " :src=" 'storage/' + post.cover_image" alt="">
                            <div class="card-body">
                                <h4>{{ post.title }}</h4>
                                 <p>{{  trimText(post.content) }}</p>
                                <a href="">Read more</a>
                            </div>
                            <div class="card-footer">
                                <div class="categories">
                                    <small v-if="post.category"> <span class="text-decoration-underline">Category:</span> {{ post.category.name }} </small>
                                </div>
                                <div class="tags" v-if="post.tags.length > 0">
                                    <small class="text-decoration-underline">Tags: </small>
                                    <ul>
                                        <li v-for="tag in post.tags" :key="tag.id">
                                            {{ tag.name }}
                                        </li>
                                    </ul>

                                </div>

                            </div>
                        </div>



                    </div>

                </div>
            </div>

            <div class="mt-4">
                <nav aria-label="Page navigation">
                    <ul class="pagination justify-content-center">
                        <li class="page-item" v-if="postsResponse.current_page > 1">
                            <a class="page-link" href="#" aria-label="Previous"
                               @click.prevent="getAllPosts(postsResponse.current_page - 1)" >
                                <span class="sr-only">Previous</span>
                            </a>
                        </li>
                        <li :class="{'page-item' : true, 'active' : page == postsResponse.current_page}" v-for="page in postsResponse.last_page" :key="page.index">

                            <a class="page-link" href="#" @click.prevent="getAllPosts(page)">
                                {{page}}
                            </a>
                        </li>
                        <li class="page-item">
                        <li class="page-item" v-if="postsResponse.current_page < postsResponse.last_page">
                            <a class="page-link" href="#" aria-label="Next"
                               @click.prevent="getAllPosts(postsResponse.current_page + 1)" >
                                <span aria-hidden="true">&laquo;</span>
                                <span class="sr-only">Next</span>
                            </a>
                        </li>
                    </ul>
                </nav>
            </div>

        </section>
        

    </div>

</template>

<script>
export default {
    name: 'App',
    components: {},
    data() {
        return{
            // posts: '',
            postsResponse: "",
            categories: "",
        }
    },

    methods: {
        getAllPosts(postPage) {
            axios
                .get('/api/posts', {
                    params: {
                        page: postPage
                    },
                })
                .then((response) => {
                    console.log(response);
                    this.postsResponse = response.data;
                    console.log(this.postsResponse)
    
                }).catch(e => {
                    console.error(e);
                });

        },
        // getAllCategories() {
        //     axios
        //         .get('/api/categories')
        //         .then((response) => {
        //             console.log(response);
        //             this.postsResponse = response.data;
    
        //         }).catch(e => {
        //             console.error(e);
        //         });

        // },       
        




        trimText(text) {
            if (text != null && text.length > 100) {
                return text.slice(0, 100) + '...'
            }
            return text;

        }

    },

    mounted() {
        console.log('mounted');
        this.getAllPosts(1);
        // this.getAllCategories();

        
    }



}

</script>