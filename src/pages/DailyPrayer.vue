<template>
  <default-container>
    <div class="bg-gradient-to-r from-emerald-700 to-teal-500 rounded-md mb-2">
      <div class="p-3 flex items-center justify-between">
        <router-link to="/">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="w-5 h-14 text-white"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M10.5 19.5L3 12m0 0l7.5-7.5M3 12h18"
            />
          </svg>
        </router-link>
        <h3 class="font-sans text-white mx-auto">Find Mosque</h3>
      </div>
    </div>

    <div id="map"></div>
  </default-container>
</template>

<script>
import DefaultContainer from "../components/DefaultContainer.vue";
import { onMounted } from "vue";

export default {
  components: { DefaultContainer },

  setup() {
    onMounted(() => {
      // Replace YOUR_API_KEY with your own Google Maps API key
      const script = document.createElement("script");
      script.src = `https://maps.googleapis.com/maps/api/js?key=AIzaSyA_svPYFjmJdX1pWuLHLlNxGPji5UPOUmk&libraries=places&callback=initMap`;
      script.async = true;
      document.head.appendChild(script);
    });

    window.initMap = () => {
      const map = new google.maps.Map(document.getElementById("map"), {
        center: { lat: -34.397, lng: 150.644 },
        zoom: 15,
      });

      const infoWindow = new google.maps.InfoWindow();

      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (position) =>
          {
            const pos =
            {
              lat: position.coords.latitude,
              lng: position.coords.longitude,
            };
            map.setCenter(pos);

            const request =
            {
              location: pos,
              radius: "5000",
              type: ["mosque"],
            };

            const service = new google.maps.places.PlacesService(map);
            const infoWindow = new google.maps.InfoWindow();

            service.nearbySearch(request, (results, status) => {
              if (status === google.maps.places.PlacesServiceStatus.OK) {
                for (let i = 0; i < results.length; i++) {
                  const place = results[i];
                  const marker = new google.maps.Marker({
                    position: place.geometry.location,
                    map,
                    title: place.name,
                    icon: {
                      url: "https://maps.google.com/mapfiles/kml/shapes/mosque_maps.png",
                      scaledSize: new google.maps.Size(32, 32),
                    },
                  });

                  marker.addListener("click", () => {
                    console.log("Marker clicked:", place.name);
                    infoWindow.setContent(`<div style="color: black;">
                    <h3>${place.name}</h3>
                    <p>${place.vicinity}</p>
                    <a href="https://www.google.com/maps/dir/?api=1&destination=${place.geometry.location.lat()},${place.geometry.location.lng()}">Get Directions</a>
                    </div>`);
                    console.log("InfoWindow content:", infoWindow.getContent());
                    infoWindow.open(map, marker);
                  });
                }
              }
            });
          },
          () => {
            handleLocationError(true, infoWindow, map.getCenter());
          }
        );
      } else {
        // Browser doesn't support Geolocation
        handleLocationError(false, infoWindow, map.getCenter());
      }

      function handleLocationError(browserHasGeolocation, infoWindow, pos) {
        infoWindow.setPosition(pos);
        infoWindow.setContent(
          browserHasGeolocation
            ? "Error: The Geolocation service failed."
            : "Error: Your browser doesn't support geolocation."
        );
        infoWindow.open(map);
      }
    };
  },
};
</script>

<style scoped>
#map {
  height: 700px;
}
</style>
