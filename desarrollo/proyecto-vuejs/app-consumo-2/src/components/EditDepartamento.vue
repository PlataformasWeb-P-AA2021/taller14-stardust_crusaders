<template>
    <div class="pt-5">
        <form @submit.prevent="update" method="post">
            <div class="form-group">
                <label for="nombre_pro">Propietario</label>
                <input
                    type="text"
                    class="form-control"
                    id="nombre_pro"
                    v-model="departamento.nombre_pro"
                    v-validate="'required'"
                    name="nombre_pro"
                    placeholder="Ingres el Propietario"
                    :class="{'is-invalid': errors.has('departamento.nombre_pro') && submitted}">
                <div class="invalid-feedback">
                    Please provide a valid name.
                </div>
            </div>

            <div class="form-group">
                <label for="costo_depar">Costo</label>
                <input
                    type="text"
                    class="form-control"
                    id="costo_depar"
                    v-model="departamento.costo_depar"
                    v-validate="'required'"
                    name="costo_depar"
                    placeholder="Ingrese el Costo"
                    :class="{'is-invalid': errors.has('departamento.costo_depar') && submitted}">
                <div class="invalid-feedback">
                    Please provide a valid costo.
                </div>
            </div>

            <div class="form-group">
                <label for="numero_cuar">Cuarto</label>
                <input
                    type="number"
                    class="form-control"
                    id="numero_cuar"
                    v-model="departamento.numero_cuar"
                    v-validate="'required'"
                    name="numero_cuar"
                    placeholder="Ingrese el número de cuartos"
                    :class="{'is-invalid': errors.has('departamento.numero_cuar') && submitted}">
                <div class="invalid-feedback">
                    Please provide a valid costo.
                </div>
            </div>

            <div class="form-group">
                <label for="edificio">Edificio</label>
                <select v-model="departamento.edificio">
                            <option v-for="e in edificiosList" :key="e.url" :value="e.url">{{ e.nombre }}</option>
                        </select>
            </div>

            <button type="submit" class="btn btn-primary">Actualizar</button>
        </form>
    </div>
</template>

<script>

import axios from 'axios';

export default {
    data() {
        return {
            departamento: {
                nombre_pro: '',
                costo_depar: '',
                numero_cuar: '',
                edificio: '',
            },
            edificiosList: [],
            submitted: false
        }
    },
    mounted() {
        this.getEdificiosList(),
        axios.get('http://127.0.0.1:8000/api/departamentos/' + this.$route.params.id + '/')
            .then( response => {
                this.departamento = response.data
        });
    },
    methods: {
      getEdificiosList() {
            axios
            .get('http://127.0.0.1:8000/api/edificios/')
            .then(response => {
                this.edificiosList = response.data['results']
            })
            .catch(error => {
                console.log(error)
            })

        },
        update: function (e) {
            this.$validator.validate().then(result => {
                this.submitted = true;
                if (!result) {
                    return;
                }
                axios.put(`http://127.0.0.1:8000/api/departamentos/${this.departamento.id}/`,
                        this.departamento
                    )
                    .then(response => {
                        this.$router.push('/departamentos');
                    })
            });
        }
    },
}
</script>
