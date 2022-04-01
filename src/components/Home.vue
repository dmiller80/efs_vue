<template>


    <!-- Welcome Text -->


    <div class="container-fuild">


        <div class="row">


            <div class="col col-12 align-items-center justify-content-center">


                <blockquote>


                    &#8220;Welcome {{validUserName}}!&#8221;


                    <footer>


                    <small>


                        <em>&mdash;Eagle Financial Services, your Midwest Financial Services Partner.</em>


                    </small>


                    </footer>


                </blockquote>


            </div>


        </div>


    </div>






    <!-- Welcome Image -->


    <div class="container-fluid">


        <div class="row align-items-center justify-content-center">


            <div class="col col-12 col-sm-10 col-md-10">


                <div class="card-group">


                <div class="card">


                  <img :src="require('@/assets/customer.jpg')" class="card-img-top" style="height: 15rem">


                  <div class="card-body">


                    <h5 class="card-title">Customers</h5>


                    <a @click="viewCustomers" class="btn btn-primary">View Details</a>


                  </div>


                </div>


                <div class="card">


                  <img :src="require('@/assets/nonstock.jpg')" class="card-img-top" style="height: 15rem">


                  <div class="card-body">


                    <h5 class="card-title">Investments</h5>


                    <a @click="" class="btn btn-primary">View Details</a>


                  </div>


                </div>


                <div class="card">


                  <img :src="require('@/assets/stocks.jpg')" class="card-img-top" style="height: 15rem">


                  <div class="card-body">


                    <h5 class="card-title">Stocks</h5>


                    <a @click="" class="btn btn-primary">View Details</a>


                  </div>


                </div>


              </div>


            </div>


        </div>


    </div>






</template>








<script>


import router from '../router'








  export default {


    name: 'Home',


    data: () => ({


      validUserName: "Guest"


    }),


    mounted() {


      this.getUser();


    },


    methods: {


      viewCustomers() {


        router.push('/customer-list');


      },




      getUser() {


        if (localStorage.getItem("isAuthenticates")


          && JSON.parse(localStorage.getItem("isAuthenticates")) === true) {


          this.validUserName = JSON.parse(localStorage.getItem("log_user"));


        }


      }


    }


  }


</script>

