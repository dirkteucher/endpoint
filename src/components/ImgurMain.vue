<template>
  <div class="imgur-main">
    <!--I prefer creating clear connections/sections between the HTML and CSS as you will see here-->
    <!--Also a big fan of creating modular web components (HTML+CSS+JS) that can easily be transferred to other frameworks outside of vue for example-->
    <!--Section one Header-->
    <header  class="header">
      <div>
        <span class="main-title">
          <img src="../assets/imgur-logo.svg" alt="Imgur Logo" class="imgur-logo">
        </span>
        <span class="search-container">

            <input
              id="search-imgur"
              type="text"
              autocomplete="off"
              class="search-input"
              v-model="searchInput"
              v-on:keyup.13="requestData()"
            >

          <img class="search-icon" alt="imgur search icon" src="../assets/search_grey.svg" v-on:click="requestData()">
        </span>
      </div>
    </header>

    <!--Section two text headings-->
    <section class="headings">
      <h1 class="list-text-description">The most viral images on the internet sorted by popularity</h1>
      <h3 class="list-secondary-text-description">Today's most popular posts</h3>
    </section>

    <!--Section three list of images-->
    <section class="image-list-container">

      <div class="image-small" v-for="(item, index) in imgurData.items">

        <div v-if="item.cover===undefined && item.animated!==true">
          <a :href="item.link" target="_blank" class="image-href">
            <img alt="" :src="item.link+'+.png+'" />
          </a>
        </div>
        <div v-if="item.cover!=undefined  && item.animated!==true">
          <a :title="item.title" :href="'https://i.imgur.com/'+item.cover+'.png'" target="_blank" class="image-href">
           <img :alt="item.title" :src="'https://i.imgur.com/'+item.cover+'b.png'" class="image"/>
          </a>
        </div>

      </div>

    </section>

  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'ImgurMain',
  data () {
    return {
      imgurData: "",
      searchInput: ""
    }
  },
  // When the app is document ready, make a request to load up some initial images for the user
  mounted: function(){
    this.searchInput = "dodgeball";
    this.requestData();
  },
  methods: {
    // Only one method added to this app which will accept search paramaters from the user to find and display images
    requestData: function(){
      const url = "https://api.imgur.com/3/gallery/t/"+this.searchInput+"/sort/window/page"
      axios.get(
        url,
        {headers: {
            "Authorization" : "Client-ID 14f62a724e524be"
          }
        }
      ).then((response) => {
        for(var x=0; x<response.data.data.items.length; x++){
        if(response.data.data.items[x].animated===true){
          response.data.data.items.splice(x,1)
        }
      }
      this.imgurData = response.data.data;

    },(error) => {
        let status = error.response.status
      })

    }
  }
}
</script>


<style lang="scss">
  @import "../scss/mixins.scss";
  // Section one Header
  .imgur-main {
    .header{
      background-color: #2C2F34;
      padding: 10px;
      height: 30px;
      .search-icon {
        width: 30px;
        cursor: pointer;
      }
      .main-title {
        margin-left: 50px;
      }
      .search-container {
        float: right;
        margin-right: 50px;
        .search-input {
          width:280px;
          height: 30px;
          position: relative;
          top: -10px;
          @include border-radius(4px);
          background-color: #595959;
          color: #dfdfdf;
          font-size: 1rem;
          padding-left: 5px;
          outline: none;
        }
      }
    }
  }

  // Section two text headings
  .imgur-main {
    height: 100%;

    .headings {
      .list-text-description {
        text-align: center;
      }

      .list-secondary-text-description {
        text-align: center;
        color: #6a6a6a;
      }
    }
  }

  // Section three list of images
  .imgur-main {
    .image-list-container {
      background-color: #2C2F34;
      margin: 0 10%;
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      .image-small {
        display: inline-block;
        height: 180px;
        position: relative;
        width: 180px;
        background: rgb(24, 24, 23) none repeat scroll 0% 0% / auto padding-box border-box;
        border: 1.98864px solid rgb(68, 68, 66);
        margin: 4px 2px;
      }

      .image-href {
        bottom: 0px;
        display: block;
        height: 180px;
        position: absolute;
        width: 180px;
        border: 0px none rgb(119, 137, 255);
        outline: rgb(119, 137, 255) none 0px;
        overflow: hidden;
      }

      .image {
        width: 180px;
      }
    }
  }


  // Media queries would normally be placed in a separate media-query.scss file alongside a mixins.scss + animations.scss and others
  // Designing the site with a mobile first approach can help to reduce the need for many media queries
  @media(max-width:536px){
    .imgur-main{
      .header {
        height: 80px;
      }
    }
    .imgur-main #search-imgur{
      width: 70%;
    }
  }
</style>
