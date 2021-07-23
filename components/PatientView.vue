<template>
    <div>
        <button class="btn btn-success float-right" @click="onNew">
            New Record
        </button>
        <h5>Patient View</h5>
        <hr>
        <form action="" @submit.prevent="onSave">
            <b-form-group label="Name:">
                <b-form-input v-model="patient.name"></b-form-input>
            </b-form-group>
            <b-form-group label="Age:">
                <b-form-input v-model="patient.age"></b-form-input>
            </b-form-group>
            <b-form-group label="Gender:">
                <b-form-input v-model="patient.gender"></b-form-input>
            </b-form-group>
            <b-form-group label="Address:">
                <b-form-input v-model="patient.address"></b-form-input>
            </b-form-group>
            <b-form-group label="Disease:">
                <b-form-input v-model="patient.disease"></b-form-input>
            </b-form-group>
            <b-form-group>
               <button class="btn btn-primary" type="submit">Save Changes</button>
                <button class="btn btn-danger" type="button" @click="onDelete" v-if="patient.id">Delete</button>
            </b-form-group>
        </form>
    </div>
</template>
<script>
export default {
    props: {
        patient: {}
    },
    methods: {
        async onSave() {
            try {
                if(!this.patient.id) {
                    await this.$axios.post('/api/patients', this.patient)
                }else {
                    await this.$axios.put('/api/patients/'+this.patient.id, this.patient)
                }
                this.$emit('saved');
            }catch(err) {
                alert(err.response.data.message)
            }
        },
        onNew() {
            this.$emit('newRecord')
        },
        async onDelete() {
            try {
                this.$axios.delete('/api/patients/'+this.patient.id)
                this.$emit('deleted')
            }catch(err) {
                alert(err.response.data.message)
            }
        }
    }
}
</script>