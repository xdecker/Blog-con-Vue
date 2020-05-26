<template>
    <section id="content">
        <hr/>
        <h1 class="subheader">{{titulo}}</h1>
        <h2>{{subtitulo}}</h2>
        <hr/>

        <h2 class="subheader">Reactividad</h2>
        <input type="text" v-model="nombre" />
        Mi nombre es: <strong>{{nombre}}</strong>

        <h2 class="subheader">Directivas Condicionales</h2>
        <input type="number" v-model="edad" />
        Mi edad es: <strong>{{edad}}</strong>

        <p class="alert-success-provisional" v-if="edad && edad >=18 && edad <= 65">
            Eres mayor de edad
        </p>

        <p class="alert-purple-provisional" v-else-if="edad && edad >=65">
            Eres de avanzada edad
        </p>

        <p class="alert-danger-provisional" v-else="">
            Eres menor de edad
        </p>


        <h2 class="subheader">Directiva Iterativa</h2>
        <ol>
            <li v-for="pelicula in peliculas" :key="pelicula">{{pelicula}}</li>
        </ol>

    </section>
</template>

<script>
    export default {
        name: "MiComponente",
        data() {
            return {
                titulo: 'Mi Componente',
                subtitulo: 'Soy el subtitulo de MiComponente',
                //con el v-model en el input se cambia esto reactivamente
                nombre: '',
                edad: 18,
                peliculas: ['Batman vs Superman', 'El senior de los anillos', 'Spiderman', 'Annabelle']
            }
        }
    }
</script>

<style scoped>

</style>
