<template
src="./CreateArticle.html" >

</template>

<script>
    import Global from "../Global";
    import axios from 'axios';
    import Article from '../models/Article';
    import swal from 'sweetalert';
    import Sidebar from "./Sidebar";
    import {required} from 'vuelidate/lib/validators';

    export default {
        name: "CreateArticle",

        data() {
            return {
                url: Global.url,
                file: '',
                submitted: false,
                article: new Article('', '', '')

            }
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
            console.log('componente montado');
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


            saveArticle(){
                this.submitted = true;
                //COMPROBAR VALIDACIONES
                this.$v.$touch();
                if(this.$v.$invalid){
                    return false;
                } else {



                    axios.post(this.url+'save', this.article)
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
                                                    'Articulo creado',
                                                    'El articulo ingresado se ha creado correctamente',
                                                    'success'
                                                );

                                                this.article = res.data.article;
                                                console.log(this.article);
                                                this.$router.push('/blog');
                                            }
                                            else {
                                                //mostrar alert de error
                                                swal(
                                                    'Articulo no creado',
                                                    'El articulo ingresado no se ha creado, intenta nuevamente',
                                                    'error'
                                                );
                                            }
                                        })
                                        .catch(err => {
                                            console.log(err);
                                        });

                                } else {
                                    swal(
                                        'Articulo creado',
                                        'El articulo ingresado se ha creado correctamente',
                                        'success'
                                    );
                                    this.article = res.data.article;
                                    console.log(this.article);
                                    this.$router.push('/blog');
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
