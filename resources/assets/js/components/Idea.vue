<template>
    <div>
        <h2 class="text-center">Captura tus ideas</h2>
        <div class="well">
            <h4>¿En que estas pensando?</h4>
            <form v-on:submit.prevent="createIdea">
                <div class="input-group">
                    <input type="text" class="form-control input-sm" maxlength="255" name="descripcion" v-model="newIdea">
                    <span class="input-group-btn">
                        <button type="submit" class="btn btn-primary btn-sm">Agregar</button>
                    </span>
                </div>
            </form>
            <hr>
            <ul class="list-unstyled" v-if="ideas != null">
                <li v-for="idea in ideas">
                    <p>
                        <small class="text-muted">
                            <em>{{ since(idea.created_at) }}</em>
                        </small>
                        - {{ idea.descripcion }}
                    </p>
                </li> 
            </ul>
            <span v-else> Cargando...</span>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import toastr from 'toastr'
    import moment from 'moment'

moment.locale('es');

    export default {
        data(){
            return {
                ideas: null,
                newIdea: null,

            }
        },
        created: function(){
            this.getIdeas();
        },
        methods: {
            since: function(d){
                return moment(d).fromNow();
            },
            getIdeas: function(){
                var url = 'mis-ideas';
                axios.get(url)
                    .then((response) => {
                        this.ideas = response.data;
                    });
            },
            createIdea: function(){
                var url = 'guardar-idea';
                axios.post(url, {
                    descripcion: this.newIdea
                }).then((response) => {
                    this.getIdeas();
                    this.newIdea = null;
                    toastr.success('Nueva Idea Agregada');

                }).catch((error) => {
                    toastr.error('Hubo un Error');
                });
            }
        },
    }
</script>
