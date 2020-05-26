<template>
    <div class="general">
        <Slider :texto="article.title" />
        <div class="center">
            <section id="content">

                <article class="article-item article-detail" v-if="article">
                    <div class="image-wrap">
                        <img :src="url+'get-image/'+article.image" :alt="article.title" v-if="article.image" />
                        <img src="../assets/default.png" alt="imagen articulo" v-else>
                    </div>

                    <h1 class="subheader">{{article.title}}</h1>
                    <span class="date">
                            {{article.date | moment('from', 'now')}}
                        </span>
                    <p>
                        {{article.content}}
                    </p>

                    <div class="clearfix"></div>

                    <router-link :to="'/editar/'+article._id" class="btn btn-warning">Editar</router-link>
                    <a @click="deleteArticle(article._id)" class="btn btn-danger">Eliminar</a>

                </article>

            </section>
            <Sidebar/>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import Global from "../Global";
    import swal from 'sweetalert';
    import Slider from "./Slider";
    import Sidebar from "./Sidebar";

    export default {
        name: "Article",
        components: {
            Slider,
            Sidebar
        },

        mounted() {
            var articleId = this.$route.params.id;
          this.getArticle(articleId);
        },

        data() {
            return {
                url: Global.url,
                article: ''
            }
        },

        methods: {
          getArticle(articleId) {
              axios.get(this.url+'/article/' +articleId)
              .then(res => {
                  if(res.data.status === 'success'){
                      console.log('articulo cargado');
                      this.article = res.data.article;
                  }
              });
          },

            deleteArticle(articleId){

              swal({
                  title: 'Estas seguro?',
                  text: 'Una vez eliminado, no podras recuperarlo!',
                  icon: 'warning',
                  buttons: true,
                  dangerMode: true
              }).then(willDelete => {
                  if(willDelete){

                      axios.delete(this.url + 'article/'+articleId)
                          .then(res => {
                              if(res.data.status === 'success'){

                                  swal(
                                      'Articulo Eliminado',
                                      'El articulo se ha eliminado correctamente',
                                      'success');
                                  this.$router.push('/blog');
                              }
                          });

                  } else {
                      swal('El articulo no se ha eliminado', {icon:'info'});
                  }
              });





            }// fin metodo deleteArticle
        }

    }
</script>

<style scoped>

</style>
