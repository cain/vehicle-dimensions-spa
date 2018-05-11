<template>
     <div v-if="info.dimensions" style="overflow-x:auto;">
        <h3>{{vehicleName}}</h3>
        <p>
            The dimensions shown below are the height, width and legnth of each variant of the {{vehicleName}}.
        </p>
        <ul>
            <li>
                Height from {{info.minimums.height}}cm to {{info.maximums.height}}cm
            </li>
            <li>
                Width from {{info.minimums.width}}cm to {{info.maximums.width}}cm.
            </li>
            <li>
                Length from {{info.minimums.length}}cm to {{info.maximums.length}}cm
            </li>
        </ul>
       <div class="table-container">
            <table class="table">
                <tr class="table-header">
                    <th>Variant</th>
                    <th>HxWxL (cm)</th>
                    <th>Kerb Weight (kg)</th>
                </tr>
                <tr
                    v-for="(dimensions, i) in info.dimensions"
                    :key="dimensions.variant_seo + i"
                >
                    <td>{{dimensions.variant}}</td>
                    <td>{{calcDimensions(dimensions)}}</td>
                    <td>{{dimensions.kerb_weight}}</td>
                </tr>
            </table>
       </div>
    </div>
</template>

<script>
export default {
    props: ['info'],
    methods: {
        calcDimensions: function(dimensions) {
            return `${dimensions.height}x${dimensions.width}x${dimensions.length}`
        }
    },
    computed: {
        vehicleName: function() {
            return `${this.info.year} ${this.info.make} ${this.info.model}`
        }
    }
}
</script>


<style lang="scss" scoped="">
.table-container {
    border: 1px solid #ebebeb;
    border-radius: 3px;
}
.table {
    min-width: 450px;
    overflow-x: scroll;
    transition: .2s;
    margin-top: 20px;
    padding: 20px;
    width: 100%;
    .table-header th {
        font-weight: 400;
        color: #1f2f3d;
        padding-bottom: 6px;      
        text-align: left;  
    }
    td {
       color: #5e6d82;
    }
}

</style>

