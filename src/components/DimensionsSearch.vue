<template>
  <div>
    <h2>Car Dimensions</h2>
    <p>Find the size of your car!</p>
    <vehicle-search-form />
    <dimensions-table :info="data.info" />
  </div>
</template>

<style lang="scss" scoped="">
.search-form {
    .search-header {
        margin: 0px  0px 6px 0px;
    }
    .search-select {
        -webkit-appearance: none;
        padding: 10px;
        margin: 6px 6px 6px 0px;
        min-width: 200px;
        background-color: #fff;
        background-image: url(https://d30y9cdsu7xlg0.cloudfront.net/png/1123247-200.png);
        background-size: 20px;
        background-repeat: no-repeat;
        background-position: center right 10px;
        &:disabled {
            background-color: rgb(244, 244, 244);
            cursor: not-allowed;
        }
    }
    color: white;    
    background: red;
    padding: 20px;
    border-radius: 3px;
    .search-info {
        display: block;
    }
    .search-button {
        padding: 10px 30px;
        background: #fff;
        border-radius: 5px;
        margin: 6px 6px 6px 0px;
    }
}
</style>

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
