<template>
  <ion-content>
    <ion-list>
      <ion-item>item 1</ion-item>
      <ion-item>item 2</ion-item>
      <ion-item>item 3</ion-item>
    </ion-list>
  </ion-content>
</template>

<script lang="ts">
import { IonContent, IonList, IonItem } from "@ionic/vue";
import { defineComponent, onMounted } from "vue";
export default defineComponent({
  name: "FilterPopover",
  components: {
    IonContent,
    IonList,
    IonItem,
  },
  setup(props: any) {
    const showProps = () => {
      console.log(props);
    };
    onMounted( () => showProps());
  },
});
</script>

<style scoped>
</style>