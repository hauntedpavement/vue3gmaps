<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="primary">
        <ion-title>vue3gmaps</ion-title>
        <ion-buttons slot="end">
          <ion-button @click="presentPopover($event)">
            <ion-icon :icon="filter"></ion-icon>
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <div id="map" />
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
  IonIcon,
  IonButton,
  IonButtons,
} from "@ionic/vue";
import { filter } from "ionicons/icons";
import { defineComponent, onMounted } from "vue";
import { Plugins, GeolocationOptions } from "@capacitor/core";
import FilterPopover from "@/components/FilterPopover.vue";
import { popoverController } from "@ionic/core";

declare const google: any;
const { Geolocation } = Plugins; //capacitor object destructuring can b used since variable is same as API name. this will only use whatever plugins we need

export default defineComponent({
  name: "Home",
  components: {
    IonContent,
    IonHeader,
    IonPage,
    IonTitle,
    IonToolbar,
    // FilterPopover,
    IonIcon,
    IonButton,
    IonButtons,
  },

  setup() {
    let map: google.maps.Map;
    let centerMarker: google.maps.Marker;
    let initLocation: google.maps.LatLng;
    const zoom = 13;
    const geolocationOpts: GeolocationOptions = {
      enableHighAccuracy: true,
    };
    const getCurrentLocation = async () => {
      const pos = await Geolocation.getCurrentPosition(geolocationOpts);
      const { latitude, longitude } = pos.coords;
      initLocation = new google.maps.LatLng(latitude, longitude);
    };
    const presentPopover = async (ev: Event) => {
      const popover = await popoverController.create({
        component: FilterPopover,
        componentProps: {
          value1: 4,
          value2: "red meat",
        },
        animated: true,
        translucent: true,
        event: ev,
        showBackdrop: false,
        backdropDismiss: false,
        cssClass: "custom-popover",
      });
      await popover.present();
    };
    const initMap = async () => {
      await getCurrentLocation();
      map = new google.maps.Map(document.getElementById("map") as HTMLElement, {
        center: initLocation,
        zoom: zoom,
        clickableIcons: false,
      });
      centerMarker = new google.maps.Marker({
        position: initLocation,
        map: map,
      });
    };
    onMounted(() => initMap());

    return {
      presentPopover,
      filter,
    };
  },
});
</script>

<style scoped>
#map {
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
}
</style>