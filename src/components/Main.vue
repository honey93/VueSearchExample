<template>
  <div>
    <Header/>
    <div class="search">
      <div class="pad-15-hor pad-15-ver search-parent">
        <div class="search-bar" >
          <b-form-input
            @input="search_text()"
            v-model="search.text"
            type="text"
            placeholder="Search by Name"
          ></b-form-input>
          <span class="search-icon">
            <i class="fas fa-search"></i>
          </span>
        </div>
        <div>
          <span class="bold">Total Likes:</span>
          {{likes.count}}
          <span class="bold">Hits:</span>
          {{likes.hit}}
        </div>
        <div>
          <b-form-select @input="sort()" v-model="search.filter" :options="options"/>
        </div>
      </div>
    </div>

    <div class="container-fluid">
      <div class="row">
        <div class="col-md-6 pad-15-ver card" v-for="wonder in wonders_data" :key="wonder.id">
          <div
            class="card-inner"
            @mouseover="show_hover(true,wonder.id)"
            @mouseout="show_hover(false,0)"
          >
            <img class="card-img" :src="wonder.imageURL">

            <div class="card-bottom pad-15-hor" v-show="!hover_flag || active_id != wonder.id">
              <div class="min-width-160">
                <span class="bold">Ratings:</span>
                <star-rating
                  :rating="wonder.ratings"
                  :show-rating="false"
                  :inline="true"
                  :star-size="15"
                ></star-rating>
              </div>
              <div class="max-width-160">
                <span class="bold">{{wonder.place}}</span>
              </div>
            </div>

            <div :class="{'card-hover':1}" v-show="hover_flag && active_id == wonder.id">
              <span
                @click="make_active(wonder.id)"
                :class="{'fas':1, 'fa-heart':1, 'absolute-star':1, 'green':check_active(wonder.id)}"
              >{{wonder.likes}}</span>
              <h5>{{wonder.place}}</h5>
              <p>{{wonder.description}}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Header from "@/components/Header.vue";

import axios from "axios";

export default {
  name: "Main",
  mounted() {
    this.hover_flag = false;

    var inside = this;

    axios
      .get("https://www.mocky.io/v2/5c7b98562f0000c013e59f07")
      .then(function(response) {
        //console.log(response);

        inside.wonders_data_actual = response.data.data;

        response.data.data.map(function(wonder) {
          inside.likes.count += wonder.likes;
        });

        inside.wonders_data_actual = inside.wonders_data_actual.map(function(
          wonder
        ) {
          wonder.active_like = false;
          return wonder;
        });
        inside.wonders_data = response.data.data;
      })
      .catch(function(error) {
        // console.log(error);
      });
  },
  data() {
    return {
      hover_flag: false,
      wonders_data_actual: [],
      wonders_data: [],
      active_id: 0,
      options: [
        { value: null, text: "Sort By" },
        { value: "a", text: "Ratings" },
        { value: "b", text: "Likes" }
      ],
      search: { filter: null, text: "" },
      likes: { count: 0, hit: 0 }
    };
  },
  methods: {
    show_hover(flag, active_id) {
      this.hover_flag = flag;
      this.active_id = active_id;
    },
    sort() {
      //console.log(this.search.filter);
      this.search.filter == "b"
        ? this.wonders_data.sort(function(a, b) {
            return b.likes - a.likes;
          })
        : this.wonders_data.sort(function(a, b) {
            return b.ratings - a.ratings;
          });
    },
    search_text() {
      //console.log(this.search.text);

      var inside = this;

      this.wonders_data = this.wonders_data_actual.filter(function(wonder) {
        if (
          wonder.place
            .toLowerCase()
            .indexOf(inside.search.text.toLowerCase()) != "-1"
        ) {
          return wonder;
        }
      });
    },
    check_active(id) {
      var flag = false;
      this.wonders_data_actual.map(function(wonder) {
        if (wonder.id == id) {
          flag = wonder.active_like;
        }
      });
      return flag;
    },
    make_active(id) {
      this.likes.hit++;
      this.wonders_data_actual = this.wonders_data_actual.map(function(wonder) {
        if (wonder.id == id) {
          wonder.active_like = !wonder.active_like;
          wonder.active_like ? wonder.likes++ : wonder.likes--;
        }

        return wonder;
      });
      var inside = this;

      inside.likes.count = 0;
      this.wonders_data_actual.map(function(wonder) {
        inside.likes.count += wonder.likes;
      });
    }
  },
  components: {
    Header
  }
};
</script>




<style scoped  lang="scss">
  @import "../styles/main.scss";
</style>


