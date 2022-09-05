<template>
  <div class="container-fluid">
    <Navbar></Navbar>
    <div class="row justify-content-center " >
      <div
        class="col-sm-12 col-md-10 border"
        style="background: white; margin-top: 3rem; border-radius: 30px; box-shadow: rgba(50, 50, 93, 0.25) 0px 50px 100px -20px, rgba(0, 0, 0, 0.3) 0px 30px 60px -30px, rgba(10, 37, 64, 0.35) 0px -2px 6px 0px inset;"
      >
        <!-- form start -->
        <div class="row justify-content-center pt-3 pb-3">
          <div class="col-sm-12 col-md-8 d-flex justify-content-center">
            <div class="input-group mb-3 mt-3">
              <input
                type="text"
                class="form-control"
                placeholder="Search Article"
                aria-label="Recipient's username"
                aria-describedby="button-addon2"
                v-model="inputdata"
                @keypress.enter="Search"
              />
              <button
                class="btn btn-outline-secondary"
                type="button"
                id="button-addon2"
                @click="Search"
              >
                Button
              </button>
            </div>
          </div>
          <!-- pitcher div start -->
          <div class="col-sm-12 col-md-8 d-flex justify-content-center" v-if="isActive">
            <div class="card">
              <div class="card-body">
                <img src="../assets/loading.gif" alt="">

              </div>
            </div>
          </div>
          <!-- pitcher div end -->
          <div class="col-sm-12 col-md-8 d-flex justify-content-center" v-for="d in business" :key="d">
            <div class="card" style="width: 100%; box-shadow: rgba(50, 50, 93, 0.25) 0px 50px 100px -20px, rgba(0, 0, 0, 0.3) 0px 30px 60px -30px, rgba(10, 37, 64, 0.35) 0px -2px 6px 0px inset;">
              <div class="card-body">
                <h2 class="card-title text-center" style="color:lightskyblue">{{d.sectionName}}</h2>
                <h5 class="card-text">
                  {{d.webTitle}}
                </h5>
                <a :href="`${d.webUrl}`" target="_blank"  class="btn btn-primary">Visit</a>
              </div>
            </div>
          </div>
        </div>

        <!-- form end -->
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Navbar from "./Navbar.vue";

export default {
  name: "Home",
  data() {
    return {
      inputdata: "",
      business: [],
      isActive:false
    };
  },
  components:{
    Navbar
  },
  methods: {
    Search() {
      this.isActive=true;
      
      axios
        .get(
          `https://content.guardianapis.com/search?q=${this.inputdata}&api-key=test`
        )
        .then((response) => {
          return response;
        })
        .then((data) => {
          var arr = data.data.response.results;
          if(arr<=0){
            this.isActive=false;
            return alert("Record Not Found !!")
            
          }
          var temp=[];
          this.isActive=false

          arr.forEach((da)=>{
            if(da.sectionName=="Film"){
              temp.push(arr);
              
            }
          })


         this.business=arr;

          console.log(temp);
          console.log(i);
          console.log(arr);
        });
        this.inputdata="";

      // `https://content.guardianapis.com/search?q=${this.inputdata}&api-key=test`
    },
  },
};
</script>

<style>
.form-control:focus {
    border-color: none;
    box-shadow: none;
    } 
    
img{
  height: 200px;
  object-fit: cover;
  object-position: center;
  border-radius: 50%;
  width:200px
  
 
}
</style>