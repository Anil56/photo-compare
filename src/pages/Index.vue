<template>
  <q-page class="flex flex-center">
    <q-card class="table-card q-mt-md q-mb-md">
      <q-card-section v-show="isPhotoList">
        <div class="row">
          <div class="col-12 q-pa-md">
            <section class="card-list">
              <article
                class="card"
                v-for="photo in photoList"
                :key="photo.id"
              >
                <div class="card-img">
                  <img
                    :src="photo.url"
                    alt="Photo Thumbnail"
                  />
                </div>

                <header style="background: solid black; text-transform: capitalize;">
                  <h6 class="photo-title">
                    <span :title="photo.title">
                      {{ photo.title }}
                    </span>
                  </h6>
                  <hr>
                  <h6 class="text-center">
                    <span :title="photo.id">
                      {{ photo.id }}
                    </span>
                  </h6>
                  <hr>
                  <p class="photo-url">
                    <a :href="photo.url">
                      {{ photo.url }}
                    </a>
                  </p>
                  <q-btn class="card-btn" :color="isSelected(photo) ? 'primary' : 'danger'" :label="isSelected(photo) ? 'Compare' : 'Remove'" @click="addToCompare(photo)" />
                </header>
              </article>
            </section>
          </div>

          <q-page-sticky position="bottom-right" :offset="[40, 30]" v-show="selected.length">
            <q-fab
              v-model="fabRight"
              hide-icon
              :label="selected.length"
              direction="up"
              color="green"
            >
              <q-fab-action color="blue" @click="compare" hide-icon label="Compare" />
            </q-fab>
          </q-page-sticky>
        </div>
      </q-card-section>
      <q-card-section v-show="!isPhotoList">
        <q-table
          title="Comparison Table"
          :columns="columns"
          :data="data"
          separator="cell"
          row-key="name"
        />
        <br>
        <br>
        <q-btn color="primary" label="BACK" @click="isPhotoList = true" />
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script>
import axios from 'axios'

const columns = [
  { name: 'name', align: 'center', label: '', field: 'name' },
  { name: 'ID', align: 'left', label: 'ID', field: 'ID' },
  { name: 'URL', align: 'left', label: 'URL', field: 'URL' },
  { name: 'Title', align: 'left', label: 'Title', field: 'Title' }
]

export default {
  name: 'PageIndex',
  async created () {
    const result = await axios.get('https://jsonplaceholder.typicode.com/photos')
    const photoList = []
    for (let index = 0; index < 50; index++) {
      photoList.push(result.data[index])
    }
    this.photoList = photoList
  },
  data () {
    return {
      columns,
      data: [],
      isPhotoList: true,
      fabRight: true,
      photoList: [],
      selected: []
    }
  },
  methods: {
    compare () {
      this.data = []
      for (let index = 0; index < this.photoList.length; index++) {
        const photo = this.photoList[index]
        if (this.selected.includes(photo.id)) {
          this.data.push({ name: 'Photo ' + (index + 1), ID: photo.id, URL: photo.url, Title: photo.title })
        }
      }
      this.fabRight = true
      this.isPhotoList = !this.isPhotoList
    },
    isSelected (photo) {
      return !this.selected.includes(photo.id)
    },
    addToCompare (photo) {
      if (this.selected.includes(photo.id)) {
        const index = this.selected.indexOf(photo.id)
        if (index > -1) {
          this.selected.splice(index, 1)
        }
      } else {
        this.selected.push(photo.id)
      }
      this.fabRight = this.selected.length > 0
    }
  }
}
</script>
<style>

.photo-title {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
}

h6 {
  margin: 0
}

.table-card {
  min-height: calc(100vh - 90px);
  width: 100%;
  min-width: 1200px;
  max-width: 1200px;
}

.card-btn {
  width: 210px;
}

.photo-url {
  font-size: 9px;
  text-transform: lowercase;
}

.ant-layout {
  display: flex;
  flex: auto;
  flex-direction: column;
  min-height: 0;
  background: #091e3c !important;
}
#components-layout-demo-top-side-2 .logo {
  float: left;
  width: 120px;
  height: 31px;
  margin: 16px 24px 16px 0;
  background: rgba(255, 255, 255, 0.3);
}

.ant-row-rtl #components-layout-demo-top-side-2 .logo {
  float: right;
  margin: 16px 0 16px 24px;
}

.site-layout-background {
  background: #000;
}

@import url("https://fonts.googleapis.com/css2?family=DM+Mono:wght@300;400;500&display=swap");

body {
  padding: 0;
  margin: 0;
  background-color: #17141d !important;
  color: white;
  font-family: "DM Mono", monospace;
}

.card-list {
  display: flex;
  padding: 1rem;
  overflow-x: scroll;
}

/* .card-list::-webkit-scrollbar {
  width: 10px;
  height: 10px;
}
.card-list::-webkit-scrollbar-thumb {
  background: #201c29;
  border-radius: 10px;
  box-shadow: inset 2px 2px 2px hsla(0, 0%, 100%, 0.25),
    inset -2px -2px 2px rgba(0, 0, 0, 0.25);
}

.card-list::-webkit-scrollbar-track {
  background: linear-gradient(90deg, #201c29, #201c29 1px, #17141d 0, #17141d);
} */

.card {
  /* cursor: pointer; */
  display: flex;
  position: relative;
  flex-direction: column;
  /* height: 300px; */
  width: 400px;
  min-width: 250px;
  padding: 1.5rem;
}

.card {
  /* ... */

  border-radius: 16px;
  background: #17141d;
  /* box-shadow: -1rem 0 3rem #000; */
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  transition: 0.2s;
}
.card-img {
  background: #17141d;
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;
  margin-top: -24px;
  transition: 0.2s;
}

.card-img img {
  width: 250px;
  border-radius: 16px 16px 0 0;
  margin: 0 -24px;
}

.card:hover {
  transform: translateY(-1rem);
}

.card:hover ~ .card {
  /* transform: translateX(150px); */
}

.card:not(:first-child) {
  margin-left: 10px;
}

.card-author {
  position: relative;
  display: grid;
  grid-template-columns: 75px 1fr;
  align-items: center;
  margin: 3rem 0 0;
}

.author-avatar img {
  display: block;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  filter: grayscale(100%);
  margin: 16px 10px;
}

.half-circle {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 60px;
  height: 48px;
  fill: none;
  stroke: #ff8a00;
  stroke-width: 8;
  stroke-linecap: round;
}

.card-header {
  margin-bottom: auto;
}

.card-header p {
  font-size: 14px;
  margin: 0 0 1rem;
  color: #7a7a8c;
}

.card-header h2 {
  font-size: 10px;
  margin: 0.25rem 0 auto;
  color: white;
  cursor: pointer;
}

.card-header h4 {
  margin: 0.25rem 0 auto;
  text-align: justify;
  color: white;
  cursor: pointer;
}

.card-header h2:hover {
  background: linear-gradient(90deg, #ff8a00, #e52e71);
  text-shadow: none;
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.tags {
  margin: 1rem 0 2rem;
  padding: 0.5rem 0 1rem;
  line-height: 2;
  margin-bottom: 0;
}

.tags a {
  font-style: normal;
  font-weight: 700;
  font-size: 0.5rem;
  color: #7a7a8c;
  text-transform: uppercase;
  font-size: 0.66rem;
  border: 3px solid #28242f;
  border-radius: 2rem;
  padding: 0.2rem 0.85rem 0.25rem;
  position: relative;
}

.tags a:hover {
  background: linear-gradient(90deg, #ff8a00, #e52e71);
  text-shadow: none;
  -webkit-text-fill-color: transparent;
  -webkit-background-clip: text;
  -webkit-box-decoration-break: clone;
  box-decoration-break: clone;
  background-clip: text;
  border-color: white;
}

.card-header h2 span {
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
