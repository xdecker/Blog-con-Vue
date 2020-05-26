<template>
    <div id="general">
        <Slider
        texto="Pagina de Prueba"
        />
        <div class="center">

            <section id="content">
                <h2 class="subheader">Pagina de prueba</h2>
                <h3>{{id_desde_url}}</h3>
                <button @click="actualizarNombre()">
                    Actualizar nombre
                </button>
                <button @click="redirigirAlBlog()">
                    Ir al Blog
                </button>
                <h4>{{nombre}}</h4>

            </section>

            <Sidebar/>

        </div>
    </div>
</template>

<script>
    import Slider from "./Slider";
    import Sidebar from "./Sidebar";



    //aqui veremos los metodos de ciclo de vida
    //mounted
    //created cuando es creado pero no termina de montarse aun
    //updated cuando un componente es actualizado
    //destroyed
    export default {
        name: "Pagina",
        components: {
            Slider,
            Sidebar
        },

        created() {
            console.log('Componente creado');
        },

        //para recoger parametros de la url
        mounted() {
            //esto se ejecuta apenas carga el componente
            this.id_desde_url = this.$route.params.id;
            console.log('El componente ya esta montado');


        },

        updated() {
            console.log('componente actualizado');
        },

        destroyed() {
            console.log('componente eliminado');
        },

        data() {
            return {
                id_desde_url: null,
                nombre: 'Xavier Decker'
            }
            //
        },


        methods: {

            actualizarNombre(){
                this.nombre = 'XAVIER DECKER WEB';
                console.log(this.nombre);
            },

            redirigirAlBlog(){
                this.$router.push('/blog');
            }

        }


    };
</script>

<style scoped>

</style>
