<template>
    <div class="pt-5">
        <form @submit.prevent="update" method="post">
            <div class="form-group">
                <label for="nombre">Nombre</label>
                <input
                    type="text"
                    class="form-control"
                    id="nombre"
                    v-model="edificio.nombre"
                    v-validate="'required'"
                    name="nombre"
                    placeholder="Ingres el nombre"
                    :class="{'is-invalid': errors.has('edificio.nombre') && submitted}">
                <div class="invalid-feedback">
                    Please provide a valid name.
                </div>
            </div>

            <div class="form-group">
                <label for="direccion">Dirección</label>
                <input
                    type="text"
                    class="form-control"
                    id="direccion"
                    v-model="edificio.direccion"
                    v-validate="'required'"
                    name="direccion"
                    placeholder="Ingres la dirección"
                    :class="{'is-invalid': errors.has('edificio.direccion') && submitted}">
                <div class="invalid-feedback">
                    Please provide a valid direccion.
                </div>
            </div>

            <div class="form-group">
                <label for="ciudad">Ciudad</label>
                <input
                    type="text"
                    class="form-control"
                    id="ciudad"
                    v-model="edificio.ciudad"
                    v-validate="'required'"
                    name="ciudad"
                    placeholder="Ingres la ciudad"
                    :class="{'is-invalid': errors.has('edificio.ciudad') && submitted}">
                <div class="invalid-feedback">
                    Please provide a valid ciudad.
                </div>
            </div>

            <div class="form-group">
                <label for="tipo_edificio">Tipo</label>
                <select v-model="edificio.tipo_edificio">
                    <option value="residencial">Residencial</option>
                    <option value="comercial">Comercial</option>
                </select>
                <div class="invalid-feedback">
                    Please provide a valid tipo_edificio.
                </div>
            </div>
            <button type="submit" class="btn btn-primary">Submit</button>
        </form>
    </div>
</template>
<script>
import axios from 'axios';

export default {
    data() {
        return {
            edificio: {
                nombre: '',
                direccion: '',
                ciudad: '',
                tipo_edificio: '',
            },
            submitted: false
        }
    },
    mounted() {
        axios.get('http://127.0.0.1:8000/api/edificios/' + this.$route.params.id + '/')
            .then( response => {
                console.log(response.data)
                this.edificio = response.data
            });
    },
    methods: {
        update: function (e) {
            this.$validator.validate().then(result => {
                this.submitted = true;
                if (!result) {
                    return;
                }
                axios.put(`http://127.0.0.1:8000/api/edificios/${this.edificio.id}/`,
                        this.edificio
                    )
                    .then(response => {
                        this.$router.push('/');
                    })
            });
        }
    },
}
</script>
