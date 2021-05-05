<template>
    <div class="container" v-if="!pending">
        sfdfdsfds
        <div v-if="pending">ПРЕЛОАДЕР</div>
    </div>
</template>

<script>
export default {
    name: "CoordinatesMap",
    data: function () {
        return {
            pending: "true",
            coordinatesData: {}
        };
    },

    created: function () {
        this.axios.get("./static/map.json").then((response) => {
        this.coordinatesData = response.data;
        this.pending = false;
        console.log(this.coordinatesData);
        });
    },
}
</script>
