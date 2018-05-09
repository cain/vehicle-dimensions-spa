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
    <select v-model="selectedModel" :disabled="canSelectModel">
        <option value="">
            Select a model.
        </option>
        <option v-for="model in data.models" :key="model.name" :value="model">
            {{model.name}}
        </option>
    </select>
    <select v-model="selectedYear" :disabled="canSelectYear">
        <option value="">
            Select a year
        </option>
        <option v-for="year in data.years" :key="'year-' + year">
            {{year}}
        </option>
    </select>
    <button @click="clear">
        Clear
    </button>
    <span v-if="loading">Loading...</span>
    <dimensions-table :info="data.info" />
  </div>
</template>

<script>
import config from '../config'
import axios from 'axios'
import DimensionsTable from './DimensionsTable'
export default {
    components: {
        DimensionsTable
    },
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
    computed: {
        canSelectModel: function() {
            return !this.selectedMake || !this.data.models.length
        },
        canSelectYear: function() {
            return !this.selectedModel || !this.data.years.length
        }
    },
    mounted() {
        // get vehicle brands
        this.loading = true;
        axios
        .get(`${config.API_URL}/vehicles/makes`)
        .then(res => this.data.makes = res.data.data)
        .finally(() => this.stopLoading())        
    },
    methods: {
        clear: function() {
            this.selectedMake = ''
            this.clearSelection()
            this.data = {
                makes: this.data.makes,
                models: [],
                years: [],
                info: {}
            }
        },
        clearSelection: function() {
            this.selectedModel = ''
            this.selectedYear = ''  
        },
        stopLoading: function() {
            // so loading doesnt flash the user.
            setTimeout(() => {
                this.loading = false;
            }, 500);
        }
    },
    watch: {
        selectedMake: function(make) {
            this.data.models = []
            this.data.info = {}  
            this.data.years = []   
            this.clearSelection()
            this.loading = true;
            // get vehicle brand models
            axios
            .get(`${config.API_URL}/vehicles/makes/${make.id}/models`)
            .then(res => this.data.models = res.data.data)
            .finally(() => this.stopLoading())
        },
        selectedModel: function(model) {
            if(!model) return;
            this.data.years = []
            this.data.info = {}            
            this.loading = true;
            // get vehicle model years
            axios
            .get(`${config.API_URL}/vehicles/makes/${this.selectedMake.id}/models/${model.id}/years`)
            .then(res => this.data.years = res.data.data)
            .finally(() => this.stopLoading())
        },
        selectedYear: function(year) {
            if(!year) return;
            this.loading = true;
            this.data.info = {}
            // get vehicle dimensions
            axios
            .get(`${config.API_URL}/dimensions?make=${this.selectedMake.name_seo}&model=${this.selectedModel.name}&year=${year}`)
            .then(res => this.data.info = res.data.data)
            .finally(() => this.stopLoading())
        }
    }
}
</script>
