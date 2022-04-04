<template>
    <div class="container-fluid">
        <!--Welcome Title -->
        <div class="row align-items-center justify-content-center">
            <div class="col col-12 align-items-center justify-content-center">
                <blockquote>
                    Welcome {{ validUserName }}!
                    <footer>
                        <small>
                            <em>&mdash;Eagle Financial Services, your Midwest Financial Services Partner.</em>
                        </small>
                    </footer>
                </blockquote>
            </div>
            <div class="col-12 col-md-10 col-lg-10 col-12 align-items-center justify-content-center">
                <div class="alert alert-success"
                     v-if="showMsg === 'new'"
                     :value="true">
                    New Stock has been added.
                </div>
                <div class="alert alert-success"
                     v-if="showMsg === 'update'"
                     :value="true">
                    Stock information has been updated.
                </div>
                <div class="alert alert-success"
                     v-if="showMsg === 'deleted'"
                     :value="true">
                    Selected stock has been deleted.
                </div>
            </div>
        </div>


        <!-- Data table -->
        <div class="row align-items-center justify-content-center">
            <div class="d-md-none" id="collapsable-card" style="width: 80%">
                <button type="button" class="btn btn-primary" @click="addNewStock" style="background-color: white; border-color: white">
                    <svg xmlns="http://www.w3.org/2000/svg" width="50" height="50" fill="#0275d8" class="bi bi-plus-circle-fill" viewBox="0 0 16 16">
                        <path d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0zM8.5 4.5a.5.5 0 0 0-1 0v3h-3a.5.5 0 0 0 0 1h3v3a.5.5 0 0 0 1 0v-3h3a.5.5 0 0 0 0-1h-3v-3z"/>
                    </svg>
                </button>
                <div class="card" v-for="stock in stocks" v-bind:key="stock">
                    <div class="card-header" :id="'heading' + stock.pk">
                        <button class="btn btn-link collapsed" data-bs-toggle="collapse" :data-bs-target="'#collapse' + stock.pk" aria-expanded="true" :aria-controls="'collapse' + stock.pk">
                            <h6 style="color: #0275d8; float: left">Customer {{stock.cust_number}} - {{stock.name}} stock</h6>
                        </button>
                    </div>


                    <div :id="'collapse' + stock.pk" class="collapse" :aria-labelledby="'heading' + stock.pk" data-bs-parent="#collapsable-card">
                        <div class="card-body">
                            <p><b>Customer #:</b> {{stock.cust_number}}</p>
                            <p><b>Symbol:</b> {{stock.symbol}}</p>
                            <p><b>Name:</b> {{stock.name}}</p>
                            <p><b>Shares:</b> {{stock.shares}}</p>
                            <p><b>Purchase Price:</b> {{stock.purchase_price}}</p>
                            <p><b>Purchase Date:</b> {{stock.purchase_date}}</p>
                            <div class="btn-group">
                                <button @click="updateStock(stock)" style="background-color: transparent; padding: 0;">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-pencil" viewBox="0 0 16 16">
                                        <path d="M12.146.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1 0 .708l-10 10a.5.5 0 0 1-.168.11l-5 2a.5.5 0 0 1-.65-.65l2-5a.5.5 0 0 1 .11-.168l10-10zM11.207 2.5 13.5 4.793 14.793 3.5 12.5 1.207 11.207 2.5zm1.586 3L10.5 3.207 4 9.707V10h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.293l6.5-6.5zm-9.761 5.175-.106.106-1.528 3.821 3.821-1.528.106-.106A.5.5 0 0 1 5 12.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.468-.325z"/>
                                    </svg></button>
                                <button @click="deleteStock(stock)" style="background-color: transparent; padding: 0;">
                                    <svg @click="deleteStock" xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-trash-fill" viewBox="0 0 16 16">
                                        <path d="M2.5 1a1 1 0 0 0-1 1v1a1 1 0 0 0 1 1H3v9a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V4h.5a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H10a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1H2.5zm3 4a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 .5-.5zM8 5a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7A.5.5 0 0 1 8 5zm3 .5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 1 0z"/>
                                    </svg></button>
                            </div>
                        </div>
                    </div>
                </div>


            </div>
            <div class="col col-12 col-md-10 d-none d-xl-block d-lg-block d-md-block">
                <table class="table table-hover" style="overflow-y: auto"
                       :headers="headers">
                    <thead>
                    <tr>
                        <th scope="col">Customer #</th>
                        <th scope="col">Symbol</th>
                        <th scope="col">Name</th>
                        <th scope="col">Shares</th>
                        <th scope="col">Purchase Price</th>
                        <th scope="col">Purchase Date</th>
                        <th scope="col">Update</th>
                        <th scope="col">Delete</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for = "stock in stocks" v-bind:key="stock">
                        <!-- Should be customer.cust_number but not working -->
                        <th scope="row">{{stock.cust_number}}</th>
                        <td>{{stock.symbol}}</td>
                        <td>{{stock.name}}</td>
                        <td>{{stock.shares}}</td>
                        <td>{{stock.purchase_price}}</td>
                        <td>{{stock.purchase_date}}</td>
                        <td @click="updateStock(stock)"><button style="background-color: transparent; padding: 0;">
                            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-pencil" viewBox="0 0 16 16">
                                <path d="M12.146.146a.5.5 0 0 1 .708 0l3 3a.5.5 0 0 1 0 .708l-10 10a.5.5 0 0 1-.168.11l-5 2a.5.5 0 0 1-.65-.65l2-5a.5.5 0 0 1 .11-.168l10-10zM11.207 2.5 13.5 4.793 14.793 3.5 12.5 1.207 11.207 2.5zm1.586 3L10.5 3.207 4 9.707V10h.5a.5.5 0 0 1 .5.5v.5h.5a.5.5 0 0 1 .5.5v.5h.293l6.5-6.5zm-9.761 5.175-.106.106-1.528 3.821 3.821-1.528.106-.106A.5.5 0 0 1 5 12.5V12h-.5a.5.5 0 0 1-.5-.5V11h-.5a.5.5 0 0 1-.468-.325z"/>
                            </svg></button>
                        </td>
                        <td @click="deleteStock(stock)"><button style="background-color: transparent; padding: 0;">
                            <svg @click="deleteStock" xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash-fill" viewBox="0 0 16 16">
                                <path d="M2.5 1a1 1 0 0 0-1 1v1a1 1 0 0 0 1 1H3v9a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V4h.5a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H10a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1H2.5zm3 4a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 .5-.5zM8 5a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7A.5.5 0 0 1 8 5zm3 .5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 1 0z"/>
                            </svg></button>
                        </td>
                    </tr>
                    </tbody>
                </table>
                <button type="button" class="btn btn-primary" @click="addNewStock">Add New Stock</button>
            </div>
        </div>
    </div>
</template>


<script>


    import router from '../router';
    import {APIService} from '../http/APIService';
    const apiService = new APIService();


    export default {
        name: "StockList",
        data: () => ({
            stocks: [],
            validUserName: "Guest",
            stockSize: 0,
            showMsg: '',
            isMobile: false,
            headers: [
                {text: 'Customer Number', sortable: false, align: 'left',},
                {text: 'Symbol', sortable: false, align: 'left',},
                {text: 'Name', sortable: false, align: 'left',},
                {text: 'Shares', sortable: false, align: 'left',},
                {text: 'Purchase Price', sortable: false, align: 'left',},
                {text: 'Purchase Date', sortable: false, align: 'left',},
                {text: 'Update', sortable: false, align: 'left',},
                {text: 'Delete', sortable: false, align: 'left',}


            ],


        }),
        mounted() {
            this.getStocks();
            this.showMessages();
        },
        methods: {
            onResize() {
                if (window.innerWidth < 600)
                    this.isMobile = true;
                else
                    this.isMobile = false;
            },
            showMessages(){
                console.log(this.$route.params.msg)
                if (this.$route.params.msg) {
                    this.showMsg = this.$route.params.msg;
                }
            },
            getStocks() {
                apiService.getStockList().then(response => {
                    this.stocks = response.data.data;
                    this.stockSize = this.stocks.length;
                    if (localStorage.getItem("isAuthenticates")
                        && JSON.parse(localStorage.getItem("isAuthenticates")) === true) {
                        this.validUserName = JSON.parse(localStorage.getItem("log_user"));
                    }
                }).catch(error => {
                    if (error.response.status === 401) {
                        localStorage.removeItem('isAuthenticates');
                        localStorage.removeItem('log_user');
                        localStorage.removeItem('token');
                        router.push("/auth");
                    }
                });
            },
            addNewStock() {
                if (localStorage.getItem("isAuthenticates")
                    && JSON.parse(localStorage.getItem("isAuthenticates")) === true) {
                    router.push('/stock-create');
                } else {
                    router.push("/auth");
                }
            },
            updateStock(stock) {
                router.push('/stock-create/' + stock.pk);
            },
            deleteStock(stock) {
                if(confirm("Do you really want to delete?")) {
                    apiService.deleteStock(stock.pk).then(response => {
                        if (response.status === 204) {
                            router.push('/stock-list/deleted/')
                            this.getStocks()
                        }
                    }).catch(error => {
                        if (error.response.status === 401) {
                            localStorage.removeItem('isAuthenticates');
                            localStorage.removeItem('log_user');
                            localStorage.removeItem('token');
                            router.push("/auth");
                        }
                    });
                }
            }
        }
    };
</script>


<style>
    button {
        padding: 1rem;
        border: 0;
        cursor: pointer;
    }
</style>

