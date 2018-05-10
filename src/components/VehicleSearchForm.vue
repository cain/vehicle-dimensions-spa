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
    
</script>
