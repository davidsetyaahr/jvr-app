<template>
  <div>
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-md-6 px-0">
          <div class="mobile p-4">
            <center>
              <img src="https://javavolcano-touroperator.com/public/assets/img/download.png" style="width:100px" alt="" srcset="">
            </center>
            <div class="mt-3 alert alert-primary font-weight-bold text-center">Welcome to JVR APP</div>
            <div class="row">
              <div class="col-6 mb-3 pr-1">
                <NuxtLink to="request-payment">
                  <div class="card-menu p-3 h-100">
                    <img src="../assets/icons/request.png" alt="" srcset="">
                    <div class="title">Request Payment List</div>
                  </div>
                </NuxtLink>
              </div>
              <div class="col-6 mb-3 pl-1">
                <NuxtLink to="">
                  <div class="card-menu p-3 h-100">
                    <img src="../assets/icons/history.png" alt="" srcset="">
                    <div class="title">Payment History</div>
                  </div>
                </NuxtLink>
              </div>
              <div class="col-6 mb-3 pr-1">
                <NuxtLink to="">
                  <div class="card-menu p-3 h-100">
                    <img src="../assets/icons/pos.png" alt="" srcset="">
                    <div class="title">Pos Transfer</div>
                  </div>
                </NuxtLink>
              </div>
              <div class="col-6 mb-3 pl-1">
                <NuxtLink to="">
                  <div class="card-menu p-3 h-100">
                    <img src="../assets/icons/submission.png" alt="" srcset="">
                    <div class="title">Expense Submission</div>
                  </div>
                </NuxtLink>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
}
</script>

<style lang="scss">
@import '../assets/css/style.scss';
</style>