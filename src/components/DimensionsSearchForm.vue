<template>
  <div class="search-form">
        <h3 class="search-header">Search Dimensions</h3>
        <small class="search-info">Select a vehicle below:</small>
        <select class="search-select" v-model="selectedMake">
        <option value="">
            Select Make*
        </option>
        <option v-for="make in data.makes" :key="make.name" :value="make">
            {{make.name}}
        </option>
        </select>
        <select class="search-select" v-model="selectedModel" :disabled="canSelectModel">
            <option value="">
                Select a model.
            </option>
            <option v-for="model in data.models" :key="model.name" :value="model">
                {{model.name}}
            </option>
        </select>
        <select class="search-select" v-model="selectedYear" :disabled="canSelectYear">
            <option value="">
                Select a year
            </option>
            <option v-for="year in data.years" :key="'year-' + year">
                {{year}}
            </option>
        </select>
        <button class="search-button" @click="clear">
            Clear
        </button>
        <span v-if="loading">Loading...</span>        
    </div>
</template>

<script>
export default {
    props: ['data', 'loading'],
    data() {
        return {
            selectedMake: '',
            selectedModel: '',
            selectedYear: '',
        }
    },
    computed: {
        canSelectModel: function() {
            return !this.selectedMake || !this.data.models.length
        },
        canSelectYear: function() {
            return !this.selectedModel || !this.data.years.length
        },
    },
    methods: {
        clear: function(year) {
            this.selectedMake = '';
            this.selectedModel = '';
            this.selectedYear = '';
            this.$emit('clear')
        }
    },
    watch: {
        selectedMake: function(make) {
            this.selectedModel = '';
            this.selectedYear = '';
            this.$emit('onSelectMake', this.selectedMake)
        },
        selectedModel: function(model) {
            this.selectedYear = ''
            this.$emit('onSelectModel',{
                make: this.selectedMake,
                model: model
            })
        },
        selectedYear: function(year) {
            this.$emit('onSelectYear', {
                make: this.selectedMake,
                model: this.selectedModel,
                year: this.selectedYear 
            })
        },
    }
}
</script>


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