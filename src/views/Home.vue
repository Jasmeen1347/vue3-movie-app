<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img src="https://a-static.besthdwallpaper.com/supervillain-joker-wallpaper-3360x1260-38724_91.jpg" alt="Naruto" class="featured-img">
        <!-- <div class="detail">
          <h3>Naruto</h3>
          <p>Lorem ipsum dolor sit amet consectetur, adipisicing elit. Cum asperiores ipsam iste ipsa animi quod nostrum perspiciatis esse optio ratione! Quam veritatis, ad facilis ipsum vel aperiam officiis reiciendis accusamus.</p>
        </div> -->
      </router-link>
    </div>
    <div class="container">
      <form @submit.prevent="searchMovies()" class="search-box">
          <input type="text" placeholder="what are you looking for?" v-model="search">
          <select v-model="type">
            <option value="">Select Type</option>
            <option value="series">Series</option>
            <option value="movie">Movie</option>
          </select>
          <input type="submit" value="Search">
      </form>

      <div class="movie-list">
        <div class="movie" v-for="movie in movies" :key="movie.imdbID">
          <router-link :to="'/movie/'+ movie.imdbID" class="movie-link">
            <div class="product-image">
              <img :src="movie.Poster" alt="Banner">
              <div class="type">{{movie.Type}}</div>
            </div>
            <div class="detail">
              <p class="year">{{ movie.Year }}</p>
              <h3>{{ movie.Title }}</h3>
            </div>
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import env from "@/env.js";
export default {
  setup(){
    const search = ref("");
    const movies = ref([]);
    const type = ref("");

    const searchMovies = () => {
        if(search.value != ""){
          fetch(`https://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}&type=${type.value}`)
          .then(response => response.json())
          .then(data => {
            movies.value = data.Search;
            // search.value = "";
          })
        }
    }

    return{
      search,
      movies,
      searchMovies,
      type
    }
  }
}
</script>

<style lang="scss">

  .home{
    .feature-card{
      position: relative;

      .featured-img{
        display: block;
        width: 100%;
        height: 500px;
        object-fit: cover;

        position: relative;
        z-index: 0;
      }

      .detail{
        position: absolute;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0,0,0,0.6);
        padding: 16px;
        z-index: 1;

        h3{
          color: #fff;
          margin-bottom: 16px;
        }
        p{
          color:#fff;
        }
      }
    }

    .search-box{
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      padding: 16px;

      input{
        display: block;
        appearance: none;
        border: none;
        outline: none;
        background: none;

        &[type="text"]{
          width: 70%;
          color: #fff;
          background-color: #496583;
          font-size: 20px;
          padding: 10px 16px;
          border-radius: 8px;
          margin-bottom: 15px;
          transition:0.4s;

          &::placeholder{
            color: #f3f3f3;
          }
          &:focus{
            box-shadow: 0px 3px 6px rgba($color: #000000, $alpha: 0.2);
          }
        }

        &[type="submit"]{
          width: 100%;
          max-width: 300px;
          background-color: #42B883;
          padding: 16px;
          border-radius: 8px;
          color: #fff;
          font-size: 20px;
          text-transform: uppercase;
          transition:0.4s;

          &:active{
            background-color: #3B8070;
          }
        }
      }

      select{
        display: inline-block;
        width: 20%;
        color: #fff;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition:0.4s;

        &:focus{
          box-shadow: 0px 3px 6px rgba($color: #000000, $alpha: 0.2);
        }
      }
    }

    .movie-list{
      display: flex;
      flex-wrap: wrap;
      margin: 0px 8px;

      .movie{
        max-width: calc((100% - 100px) / 4);
        flex: 1 1 calc((100% - 100px) / 4);
        padding: 16px 8px;

        .movie-link{
          display: flex;
          flex-direction: column;
          height: 100%;

          .product-image{
            position: relative;
            display: block;

            img{
              display: block;
              width: 100%;
              height: 275px;
              // object-fit: cover;
            }

            .type{
              position: absolute;
              padding: 8px 16px;
              background-color: #42B883;
              color: #fff;
              bottom:16px;
              left: 0;
              text-transform: capitalize;
            }
          }

          .detail{
            background-color: #496583;
            padding: 16px 8px;
            flex: 1 1 100%;
            border-radius: 0px 0px 8px 8px;

            .year{
              color: #aaa;
              font-size: 14px;
            }

            h3{
              color: #fff;
              font-weight: 600;
              font-size: 18px;
            }
          }
        }
      }
    }
  }

  .container{
    width: 80%;
    margin: 0 auto;
  }
</style>
