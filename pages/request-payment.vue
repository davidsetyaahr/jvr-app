<template>
    <div>
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" integrity="sha512-iecdLmaskl7CVkqkXNQ/ZH/XLlvWZOJyj7Yy7tcenmpD1ypASozpmT/E0iPtmFIB46ZmdtAc9eNBvH0H/ZpiBw==" crossorigin="anonymous" referrerpolicy="no-referrer" />        
        <div class="container">
            <div class="row justify-content-center">
                <div class="col-md-6 px-0">
                    <div class="mobile">
                        <div class="p-4 top-nav d-flex align-items-center">
                            <NuxtLink to="/" class="btn btn-primary btn-sm">
                                <img src="../assets/icons/arrow.png" style="width:20px" alt="" srcset="">
                            </NuxtLink>
                            <span class="mx-auto fs-18 font-weight-bold">Request Payment</span>
                        </div>
                        <div class="p-4">
                            <div v-if="msg !== ''">
                                <b-alert class="font-weight-bold" variant="primary" show dismissible>
                                    {{msg}}
                                </b-alert>
                            </div>
                            <div v-for="(data,key) in requestPayment" :key="key" class="card border-0 shadow-sm p-3 mb-3">
                                <div class="d-flex justify-content-between align-items-center">
                                    <h6 class="font-weight-bold my-0 text-primary"><span class="fa fa-user mr-1"></span> {{data.tw_calculation.customer}}</h6>
                                    <a href="" class="btn btn-default btn-sm border" @click.prevent="payment(data.id)"><span class="fa fa-credit-card text-success mr-1"></span> Pay</a>
                                </div>
                                <hr>
                                <div class="d-flex justify-content-between">
                                    <span> {{data.pos_transfer.name}}</span>
                                    <span class="text-primary"> {{rupiah(data.total)}}</span>
                                </div>
                                <hr>
                                <div class="d-flex align-items-center justify-content-between">
                                    <span> {{data.pos_transfer.bank_name}} <br> {{data.pos_transfer.rekening_number}}</span>
                                    <span class="text-primary"> {{data.pos_transfer.rekening_name}}</span>
                                </div>
                                <hr>
                                <span class="font-weight-bold">{{data.description}}</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div v-if="isPayment" class="payment">
            <div class="content p-4">
                <label for="" class="fs-15 font-weight-bold">Upload File</label>
                <input ref="file" type="file" name="" class="form-control">
                <p class="my-2 text-secondary">Request payment will be removed after this step.</p>
                <hr>
                <div class="d-flex justify-content-between">
                    <button class="btn btn-sm btn-secondary" @click.prevent="isPayment = false"><span class="fa fa-times mr-1"></span> Cancel</button>
                    <button class="btn btn-sm btn-primary" @click.prevent="submit">
                        <span v-if="loading" class="spinner-border spinner-border-sm" role="status">
                            <span class="sr-only">Loading...</span>
                        </span>                 
                        <span v-else>
                            <span class="fa fa-save mr-1"></span> Submit
                        </span>
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            loading : false,
            isPayment : false,
            selectedId : null,
            msg : '',
            requestPayment : [],
        }
    },
    mounted(){
        this.getData()
    },
    methods:{
        async getData() {
            const data = await this.$axios.$get('http://192.168.1.8:8000/api/internal/request-payment')
            this.requestPayment = data.data
        },
        payment(id){
            this.isPayment = true         
            this.selectedId = id
        },
        rupiah(number){
            return new Intl.NumberFormat("id-ID", {
            style: "currency",
            currency: "IDR"
            }).format(number);
        },
        async submit(){
            const fileUpload = this.$refs.file.files[0]
            if(typeof fileUpload !== 'undefined'){
                this.loading = true
                const formData = new FormData();
                formData.append('id',this.selectedId);
                formData.append('file',fileUpload);
                await this.$axios.$post('http://192.168.1.8:8000/api/internal/request-payment',formData,
                {
                    headers: {
                        'Content-Type': 'multipart/form-data'
                    }
                }
                ).then((res) => {
                    if(res.status === 'success'){
                        this.msg = 'Payment Success!'
                        this.getData()
                        this.loading = false
                        this.isPayment = false
                    }
                })
            }
        }
    },
}
</script>
<style lang="scss">
@import '../assets/css/style.scss';
</style>

<style lang="scss" scoped>
.dropdown-toggle::after{
    content : inherit;
}
.btn-dropdown-toggle{
    padding:0;
}
.payment{
    position: fixed;
    top : 0;
    left : 0;
    width : 100%;
    height : 100%;
    background: rgba($color: #000000, $alpha: .5);
    display: flex;
    justify-content: center;
    align-items: center;
    .content{
        background: white;
        width : 90%;
        border-radius: 30px;
    }
}
</style>