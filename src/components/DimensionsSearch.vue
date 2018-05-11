<template>
  <div>
    <dimensions-search-form
        :data="data"
        :loading="loading"
        v-on:clear="clear"        
        v-on:onSelectMake="selectMake"
        v-on:onSelectModel="selectModel"
        v-on:onSelectYear="selectYear"
    />
    <span v-if="loading">Loading...</span>
    <dimensions-table :info="data.info" />
    <p v-if="errMsg">{{errMsg}}</p>
  </div>
</template>

<script>
import config from '../config'
import axios from 'axios'
import DimensionsTable from './DimensionsTable'
import DimensionsSearchForm from './DimensionsSearchForm'
export default {
    components: {
        DimensionsTable,
        DimensionsSearchForm
    },
    data() {
        return {
            loading: false,
            errMsg: '',
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
        this.getVehicleBrands()  
    },
    methods: {
        clear: function() {
            this.data = {
                makes: this.data.makes,
                models: [],
                years: [],
                info: {}
            }
        },
        stopLoading: function() {
            // so loading doesnt flash the user.
            setTimeout(() => {
                this.loading = false;
            }, 200);
        },
        selectMake: function(make) {
            // clear state
            this.clear()
            this.loading = true;
            this.errMsg = '';

            // get vehicle brand's models
            axios
            .get(`${config.API_URL}/vehicles/makes/${make.id}/models`)
            .then(res => this.data.models = res.data.data)
            .catch(err => this.errMsg = {...err}.response.data.errorMsg)
            .finally(() => this.stopLoading())
        },
        selectModel: function({make, model}) {
            if(!model) return;

            // clear state
            this.data.years = []
            this.data.info = {}            
            this.loading = true;
            this.errMsg = '';

            // get vehicle brand's models years
            axios
            .get(`${config.API_URL}/vehicles/makes/${make.id}/models/${model.id}/years`)
            .then(res => this.data.years = res.data.data)
            .catch(err => this.errMsg = {...err}.response.data.errorMsg)            
            .finally(() => this.stopLoading())
        },
        selectYear: function({make, model, year}) {
            if(!year) return;

            // clear state
            this.loading = true;
            this.errMsg = '';
            this.data.info = {}

            // get selected vehicle's dimensions
            axios
            .get(`${config.API_URL}/dimensions?make=${make.name_seo}&model=${model.name_seo}&year=${year}`)
            .then(res => this.data.info = {...res.data.data, year})
            .catch(err => this.errMsg = {...err}.response.data.errorMsg)
            .finally(() => this.stopLoading())
        },
        getVehicleBrands: function() {
            this.loading = true;
            this.errMsg = '';

            axios
            .get(`${config.API_URL}/vehicles/makes`)
            .then(res => this.data.makes = res.data.data)
            .catch(err => this.errMsg = 'Whoops, something went wrong!')            
            .finally(() => this.stopLoading())      
        }
    },
}
</script>