<template>
    <div id="general">
        <Slider
        texto="Bienvenido al Curso de Vue con Víctor Robles de victorroblesweb.es"
        home="true"
        />

        <div class="center">
            <section id="content">
                <h2 class="subheader">Últimos artículos</h2>

                <!--Listado articulos-->
                <div id="articles">

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
    import Global from "../Global";
    import axios from 'axios';
    import Articles from "./Articles";
    import Slider from "./Slider";
    import Sidebar from "./Sidebar";

    export default {
        name: "LastArticles",

        mounted() {
            this.getLastArticles();
        },

        data() {
            return {
                url: Global.url,
                articles: null
            }
        },

        components: {
            Sidebar,
            Slider,
            Articles
        },

        methods: {
            getLastArticles() {
                axios.get(this.url+'articles/5')
                .then(res => {
                    if(res.data.status === 'success'){
                        this.articles = res.data.articles;
                    }
                });
            }
        }
    }
</script>

<style scoped>

</style>
