<template>
  <v-card>
    <v-card-title class="align-center justify-center text-center">
      Поиск изображений in flickr
    </v-card-title>
    <v-card-content>
      <v-text-field v-model="tag" label="Запрос"></v-text-field>
      <v-text-field v-model="value" label="Количество:"></v-text-field>
    </v-card-content>
    <v-card-actions>
      <v-btn
        variant="outlined"
        @click="search"
      >Поиск
      </v-btn>
    </v-card-actions>
    <v-container>
      <v-col style="padding-bottom: 100px"
          cols="4">
      <v-card
          class="ma-8"
              v-for="image in images" :key="image.id">
        <v-card-title>Имя: {{image.ownername}}</v-card-title>
        <v-card-subtitle>Дата: {{image.datetaken}} </v-card-subtitle>
        <v-card-text>
          <v-img height="500"
                 width="500"
                 :src="image['url_n']"></v-img>
        </v-card-text>
        <v-card-actions>
          <v-spacer>
          </v-spacer>
          <v-btn size="small" color="red" variant="text" icon="mdi-heart"

                 @click="image.likes +=1"
          >
            <v-card>
            </v-card>
          </v-btn>
          <div>{{image.likes}}</div>

          <v-btn
          :icon="image.isShow ? 'mdi-chevron-up' : 'mdi-chevron-down'"
              @click="image.isShow = !image.isShow">
          </v-btn>
        </v-card-actions>
        <v-expand-transition>
          <div v-show="image.isShow">
            <v-divider></v-divider>
            <v-card-text>
              <p> owner: {{image.owner}}</p>
              <p> views: {{image.views}}</p>
              <p>  id: {{image.id}}</p>
            </v-card-text>
          </div>
        </v-expand-transition>
      </v-card>
      </v-col>
    </v-container>


  </v-card>
</template>

<script>

import axios from "axios";

export default
    {
      name: 'App',
      data() {
        return {
          show: false,
          tag: '',
          images: [
            {isShow: false, likes: 0}
          ],
          value: 5,
        };
      },
      methods: {

        search() {
          this.fetchImages();
        },
        fetchImages() {
          axios.get('https://api.flickr.com/services/rest', {
            params: {
              method: 'flickr.photos.search',
              api_key: 'e5701b4511946633fcd188a4c28acad2',
              tags: this.tag,
              extras: 'url_n, owner_name, date_taken, views',
              page: 1,
              format: 'json',
              nojsoncallback: 1,
              per_page: this.value,
            },
          })
              .then((response) => {
                this.images = response.data.photos.photo;
                this.loading = false;
              });
        },
      },
      mounted() {
        this.fetchImages();
      },
    }
</script>
