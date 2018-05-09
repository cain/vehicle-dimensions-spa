<template>
      <table style="width:100%" v-if="info.dimensions">
        <tr>
            <th>Variant</th>
            <th>HxWxL (cm)</th>
            <th>Kerb Weight (kg)</th>
        </tr>
        <tr v-for="(dimensions, i) in info.dimensions" :key="dimensions.variant_seo + i" v-if="dimensions.variant">
            <td>{{dimensions.variant}}</td>
            <td>{{dimensions.height}}x{{dimensions.width}}x{{dimensions.length}}</td>
            <td>{{dimensions.kerb_weight}}</td>
        </tr>
    </table>
</template>

<script>
export default {
    props: ['info']
}
</script>


<style scoped="">
th {
    text-align: left;
}
</style>

