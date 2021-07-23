<template>
    <div>
        <NavBar />
        <div class="container">
            <h1>My Patients</h1>
            <div class="row">
                <div class="col-6">
                    <h5>List of Patients</h5>
                    <hr>
                    <ul class="list-group">
                        <li class="list-group-item btn-li"  v-for="patient in patients" :key="patient.id" @click="onSelected(patient)">
                            {{patient.name}} <span class="float-right">{{patient.age}}</span>
                        </li>
                    </ul>
                </div>
                <div class="col-4">
                    <PatientView :patient="selectedPatient" @saved="onChange" @newPatient="onNew" @deleted="onChange" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            patients: [],
            selectedPatient: {}
        }
    },
    methods: {
        async getMyPatients() {
            await this.$axios.get('/api/patients')
            .then((res)=>{
                if(res.status==200) {
                    this.patients = res.data
                }
            })
        },
        onChange() {
            this.getMyPatients()
            this.selectedPatient = {}
        },
        onSelected(patient) {
            this.selectedPatient = patient;
        },
        onNew() {
            this.selectedPatient = {}
        },
    },
    created() {
        this.getMyPatients()
    }
}
</script>