<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>
          Photo Gallery
        </ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-grid>
        <div class="ion-text-right ion-margin-end ion-margin-bottom">  
          <ion-icon :icon="image" class="ion-padding-top ion-padding-end" />          
          <ion-toggle :checked="false" @ionChange="changeSize()" />
          <ion-icon :icon="images" class="ion-padding-top ion-padding-start" />
        </div>
        <ion-row>
          <ion-col :size="size" :key="photo.filepath" v-for="photo in photos">
            <ion-img :src="photo.webviewPath" @click="showActionSheet(photo)" class="photo"></ion-img>
          </ion-col>
        </ion-row>
      </ion-grid>
      <ion-fab vertical="bottom" horizontal="center" slot="fixed">
        <ion-fab-button @click="takePhoto()">
          <ion-icon :icon="camera" />
        </ion-fab-button>
      </ion-fab>

    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { 
  actionSheetController,
  IonToggle,
  IonPage,
  IonHeader,
  IonFab,
  IonFabButton,
  IonIcon,
  IonToolbar,
  IonTitle,
  IonContent,
  IonGrid,
  IonRow,
  IonCol,
  IonImg,
} from '@ionic/vue';
import { camera, trash, close, image, images } from 'ionicons/icons';
import { usePhotoGallery, UserPhoto } from '@/composables/usePhotoGallery';
const { takePhoto, photos, deletePhoto } = usePhotoGallery();

const showActionSheet = async (photo: UserPhoto) => {
  const actionSheet = await actionSheetController.create ({
    header: 'Photos',
    buttons: [
      {
        text: 'Delete',
        role: 'destructive',
        icon: trash,
        handler: () => {
          deletePhoto (photo)
        }
      },
      {
        text: 'Cancel',
        role: 'cancel',
        icon: close,
        handler: () => {
        } 
      }
    ]
  });
  await actionSheet.present();
}

const size = ref('12');
const changeSize = () => {
  size.value === '12' ? size.value = '3' : size.value = '12'; 
}

</script>

<style>

.photo:active, .photo:hover {
  outline: solid 5px #1554e9;
}

</style>
