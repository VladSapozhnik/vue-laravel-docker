<template>
    <div class="map__page" v-if="!pending">
        <GmapMap
            :center="{lat:49.016209109037355, lng:31.593056998693335}"
            :zoom="6"
            map-type-id="terrain"
            class="map"
            >
            <GmapMarker
                :key="index"
                v-for="(marker, index) in markers"
                :position="marker.position"
                :clickable="true"
                :draggable="true"
                @click="toggleInfoWindow(marker, index)"
            />

            <gmap-info-window
                :options="infoOptions"
                :position="infoWindowPos"
                :opened="infoWinOpen"
                @closeclick="infoWinOpen=false"
            >
            <div v-html="infoContent"></div>
            </gmap-info-window>
        </GmapMap>
        <div v-if="pending">PRELOADER</div>
    </div>
</template>

<script>
export default {
    name: "CoordinatesMap",
    data: function () {
        return {
            pending: "true",
            markers: [],
            infoContent: '',
            infoWindowPos: {
                lat: 0,
                lng: 0
            },
            infoWinOpen: false,
            infoOptions: {
                pixelOffset: {
                    width: 0,
                    height: -35
                },
            },
        };
    },

    created: function () {
        this.axios.get("./static/map.json").then((response) => {
        this.markers = response.data;
        this.pending = false;
        console.log(this.markers);
        });
    },

    mounted() {
    },

    methods: {
       toggleInfoWindow: function (markers, idx) {
        this.infoWindowPos = markers.position;
        this.infoContent = this.getInfoWindowContent(markers);

        if (this.currentMidx == idx) {
          this.infoWinOpen = !this.infoWinOpen;
        } else {
          this.infoWinOpen = true;
          this.currentMidx = idx;
        }
      },
      getInfoWindowContent: function (markers) {
        return (
            `<div class="map__info">
                <div class="map__info-title">
                    <span style="font-weight: bold;">Name: </span>
                    ${markers.title}
                </div>
                <div class="map__info-text">
                    <span style="font-weight: bold;">Info:  </span>
                    ${markers.description}
                    <br>
                </div>
            </div>`
        );
      },
    }
}
</script>
