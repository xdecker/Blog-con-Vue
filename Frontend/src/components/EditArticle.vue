<template
        src="./CreateArticle.html">
    
</template>

<script>
    import Global from "../Global";
    import Article from "../models/Article";
    import {required} from "vuelidate/lib/validators";
    import Sidebar from "./Sidebar";
    import axios from "axios";
    import swal from "sweetalert";

    export default {
        name: "EditArticle",

        data() {
            return {
                url: Global.url,
                file: '',
                submitted: false,
                article: new Article('', '', ''),
                isEdit: true
            };
        },

        validations: {

            article: {

                title:{
                    required
                },
                content:{
                    required
                }
            }
        },


        mounted() {
            var articleId = this.$route.params.id;
            this.getArticle(articleId);
        },

        components: {
            Sidebar,
        },

        methods: {

            fileChange(){
                //recoger fichero
                this.file = this.$refs.file.files[0];
                console.log(this.file);
            },

            getArticle(articleId) {
                axios.get(this.url+'/article/' +articleId)
                    .then(res => {
                        if(res.data.status === 'success'){
                            console.log('articulo cargado');
                            this.article = res.data.article;
                        }
                    })
            },


            saveArticle(){
                this.submitted = true;
                var articleId = this.$route.params.id;
                //COMPROBAR VALIDACIONES
                this.$v.$touch();
                if(this.$v.$invalid){
                    return false;
                } else {



                    axios.put(this.url+'article/'+ articleId, this.article)
                        .then(res=>{
                            if(res.data.status === 'success'){

                                //ver si se envio un archivo
                                if(this.file !== null && this.file !== undefined && this.file !== ''){

                                    //adjuntar el fichero con formdata
                                    const formData = new FormData();
                                    formData.append(
                                        'image',
                                        this.file,
                                        this.file.name
                                    );
                                    var articleId = res.data.article._id;
                                    axios.post(this.url + 'upload-image/'+ articleId, formData)
                                        .then(response =>{
                                            if(response.data.article){

                                                swal(
                                                    'Articulo actualizado',
                                                    'El articulo se ha editado correctamente',
                                                    'success'
                                                );

                                                this.article = res.data.article;
                                                console.log(this.article);
                                                this.$router.push('/articulo/'+ this.article._id);
                                            }
                                            else {
                                                //mostrar alert de error
                                                swal(
                                                    'Articulo no actualizado',
                                                    'El articulo no se ha editado, intenta nuevamente',
                                                    'error'
                                                );
                                            }
                                        })
                                        .catch(err => {
                                            console.log(err);
                                        });

                                } else {
                                    swal(
                                        'Articulo actualizado',
                                        'El articulo se ha editado correctamente',
                                        'success'
                                    );
                                    this.article = res.data.article;
                                    console.log(this.article);
                                    this.$router.push('/articulo/'+this.article._id);
                                }



                            }// fin if primer then
                        })
                        .catch(error => {
                            console.log(error);
                        });

                }//fin else
            }
        }
    }
</script>

<style scoped>

</style>
