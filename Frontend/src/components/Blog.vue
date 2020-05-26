<template>
    <div id="general">
        <Slider
        texto="Blog"
        />
        <div class="center">
            <section id="content">
                <h1 class="subheader">Blog</h1>

                <div id="articles" v-if="articles">
                    <Articles
                    :articles="articles"
                    />

                </div>


            </section>
            <Sidebar/>
        </div>
    </div>
</template>

<script>

    //para peticiones
    import axios from 'axios';
    import Global from "../Global";
    import Articles from "./Articles";

    import Slider from "./Slider";
    import Sidebar from "./Sidebar";


    export default {
        name: "Blog",
        components: {
            Slider,
            Sidebar,
            Articles
        },

        mounted() {
            //los articulos cargan al momento de montarse el comp
            this.getArticles();
        },

        data() {
            return {
                url: Global.url,
                articles: null
            }
        },

        methods : {
            getArticles() {
                axios.get(this.url+'articles')
                    .then(res =>{
                        if(res.data.status === 'success')
                        {
                            this.articles = res.data.articles;
                            console.log(this.articles);
                        }

                    });
            }
        }
    }
</script>

<style scoped>

</style>
