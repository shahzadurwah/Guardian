<template>
  <div class="container-fluid">
    <div class="row justify-content-center">
      <!-- col 1 start -->
      <div class="col-8 text-center mt-5 white">
        <div class="title">
          <h1>Guardian Search</h1>
        </div>
        <div class="inputdata">
          <form>
            <i class="fa fa-search"></i>
            <input type="text" v-model="inputdata" />
            <button class="btn btn-primary radius pr-3" @click.prevent="Search">
              Search
            </button>
          </form>
        </div>
        <div class="image loading" v-if="dataLoading">
          <!-- <img src="../assets/load.gif" class="img-fluid" /> -->
        </div>
      </div>
      <!-- col 2 start -->
      <div class="col-8 pt4 white" v-show="dataLoad">
        <div class="subtitle">
          <h3>{{ notFound }}</h3>
          <hr class="hrb" />
        </div>
      </div>
      <div class="col-8 pt4 white" v-show="dataLoad">
        <div class="subtitle subtitle-main" v-for="da in arrayData" :key="da">
          <h3 class="text-center">{{ da.sectionName }}</h3>
          <div class="content">
            <div class="para">
              <p>
                <a :href="`${da.webUrl}`" target="_blank">{{ da.webTitle }}</a>
              </p>
            </div>
            <div class="date text-gray">
              <small>{{ formateDate(da.webPublicationDate) }}</small>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from 'moment'
export default {
  name: "Home",
  data() {
    return {
      inputdata: "",
      dataLoading: false,
      dataLoad: false,
      notFound: "Result",
      notFoundfal: false,
      arrayData: [],
    };
  },
  methods: {
    formateDate(value) {
      if (value) {
        return moment(String(value)).format('MM/DD/YYYY');
      }
    },
    async Search() {
      var data = [];

      this.dataLoading = true;

      var data = await axios.get(
        `https://content.guardianapis.com/search?q=${this.inputdata}&api-key=test`
      );

      if (data) {
        this.dataLoading = false;

        this.dataLoad = true;
        this.arrayData = data.data.response.results;
        this.notFound = "Result";
        console.log(this.arrayData);
      }
      if (this.arrayData == "" || this.arrayData == null) {
        this.notFound = "Record Not Found";
      }
      // this.notFound=''
      this.inputdata = "";
    },
  },
};
</script>

<style>
.loading {
  background: transparent
    url("https://miro.medium.com/max/882/1*9EBHIOzhE1XfMYoKz1JcsQ.gif") center
    no-repeat;
  height: 400px;
  width: 400px;
}
.image {
  width: 200px;
  height: 150px;
  margin: 1rem auto;
}
.image img {
  width: 100%;
  height: 100%;
  background-size: cover;
  background-position: center;
}
.white {
  background-color: #fff;
  border-radius: 10px;
}
.white h1 {
  font-size: 60px;
  padding: 20px 0;
  color: blue;
}
.inputdata {
  /* border: 2px solid black; */
  max-width: 400px;
  margin: 1rem auto;
  justify-content: center;
}
form {
  display: flex;
  border: 2px solid lightblue;
  border-radius: 10px;
  padding: 5px;
}

input {
  flex-grow: 2;
  border: none;
  color: blue;
  font-size: 20px;
}
input:focus {
  outline: none;
}
i {
  padding-right: 10px;
  padding-left: 10px;
  font-size: 10px;
  text-align: center;
  margin-top: 10px;
}

.subtitle {
  padding: 10px 20px;
}
.subtitle h3 {
  color: gray;
}
.hrb {
  border: 1px solid gray;
}
.subtitle-main h3 {
  background-color: lightblue;
  color: rgba(0, 0, 255, 0.511);
  padding: 10px 0;
  border-radius: 10px;
}
.content {
  display: flex;
  justify-content: space-between;
  padding: 0px 20px;
  margin-top: 1rem;
  align-items: center;
  border-radius: 10px;
  background-color: rgba(76, 185, 221, 0.727);
}
.content .para {
  margin-top: 1rem;
  color: blue;
  font-size: 15px;
  font-weight: bold;
}
</style>