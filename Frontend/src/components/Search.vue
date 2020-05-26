<template>
    <div class="general">
        <Slider :texto="'Busqueda: ' + searchString " />
        <div class="center">
            <section id="content">
                <h1 class="subheader">Articulos encontrados</h1>
                <div id="articles" v-if="articles">
                    <Articles
                    :articles="articles"
                    />
                </div>



                <div v-else>
                    <h2>No hay articulos que coincidan con la busqueda</h2>
                </div>

            </section>
            <Sidebar/>
        </div>
    </div>
</template>

<script>
    import Slider from "./Slider";
    import Sidebar from "./Sidebar";
    import axios from "axios";
    import Global from "../Global";
    import Articles from "./Articles";

    export default {
        name: "Search",

        mounted() {
            //pasar el campo a buscar
            this.searchString = this.$route.params.searchString;
            this.getArticlesBySearch(this.searchString);
        },

        components: {
            Slider,
            Sidebar,
            Articles
        },

        data() {
            return {
                url: Global.url,
                articles: null,
                searchString: ''
            }
        },

        methods: {
            getArticlesBySearch(searchString) {
                axios.get(this.url+'search/'+searchString)
                .then(res=> {
                    if(res.data.status === 'success'){
                        this.articles = res.data.articles;
                        console.log(this.articles);
                    }
                })
            }
        }



    }
</script>

<style scoped>

</style>
