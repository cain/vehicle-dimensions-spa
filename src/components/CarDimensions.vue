<template>
  <div>
    <h2>Car Dimensions</h2>
    <p>Find the size of your car!</p>
    <select v-model="selectedMake">
        <option value="">
            Select Make*
        </option>
        <option v-for="make in data.makes" :key="make.name" :value="make">
            {{make.name}}
        </option>
    </select>
    <select v-model="selectedModel" :disabled="!selectedMake || !data.models.length">
        <option value="">
            Select a model.
        </option>
        <option v-for="model in data.models" :key="model.name" :value="model">
            {{model.name}}
        </option>
    </select>
    <select v-model="selectedYear" :disabled="!selectedModel || !data.years.length">
        <option value="">
            Select a year
        </option>
        <option v-for="year in data.years" :key="year">
            {{year}}
        </option>
    </select>
    <button @click="clear">
        Clear
    </button>
    <br />
    <small v-if="loading">Loading...</small>

    <div v-for="dimensions in data.info.dimensions">
        {{dimensions}}
    </div>
  </div>
</template>

<script>
import config from '../config'
import axios from 'axios'
export default {
    data() {
        return {
            loading: false,
            selectedMake: '',
            selectedModel: '',
            selectedYear: '',
            data: {
                makes: [],
                models: [],
                years: [],
                info: {}
            }
        }
    },
    mounted() {
        // get vehicle brands
        this.loading = true;
        axios
        .get(`${config.API_URL}/vehicles/makes`)
        .then(res => this.data.makes = res.data.data)
        .finally(() => this.loading = false)        
    },
    methods: {
        clear: function() {
            this.selectedMake = ''
            this.selectedModel = ''
            this.selectedYear = ''
        }
    },
    watch: {
        selectedMake: function(make) {
            if(!make){
                this.data.models = []
                this.data.years = []
                return;
            }
            this.data.models = []
            this.loading = true;
            // get vehicle brand models
            axios
            .get(`${config.API_URL}/vehicles/makes/${make.id}/models`)
            .then(res => this.data.models = res.data.data)
            .finally(() => this.loading = false)
        },
        selectedModel: function(model) {
            if(!model) return;
            this.data.years = []
            this.loading = true;
            // get vehicle model years
            axios
            .get(`${config.API_URL}/vehicles/makes/${this.selectedMake.id}/models/${model.id}/years`)
            .then(res => this.data.years = res.data.data)
            .finally(() => this.loading = false)
        },
        selectedYear: function(year) {
            if(!year) return;
            this.loading = true;
            this.data.info = {}
            // get vehicle dimensions
            axios
            .get(`${config.API_URL}/dimensions?make=${this.selectedMake.name_seo}&model=${this.selectedModel.name}&year=${year}`)
            .then(res => this.data.info = res.data.data)
            .finally(() => this.loading = false)
        }
    }
}
</script>
