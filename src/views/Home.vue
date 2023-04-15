<template>
  <div class="home col-lg-5 col-md-7 col-sm-8 col-11">
    <h1 class="mb-3">Welcome to Domain Blacklist checker</h1>
    <h2 class="mb-5">Enter Your Domain Below To Check If It's Blacklisted</h2>

    <div class="input-group mb-3">
      <input v-model="domain" type="text" class="form-control" placeholder="Enter Your Domain Name" aria-label="Domain Name to Check" aria-describedby="basic-addon2">
      <div class="input-group-append">
        <button class="btn btn-outline-secondary" type="button" @click="check">Check</button>
      </div>
    </div>

    <div v-if="submitted_once" class="card">
      <div v-if="is_blacklisted" class="card-body">
        Unfortunately, Your Domain Name Is Blacklisted
      </div>
      <div v-else class="card-body">
        Congratulations, Your Domain Name Is Not Blacklisted
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  components: {
    HelloWorld
  },
  data() {
    return {
      domain: '',
      is_blacklisted: false,
      submitted_once: false,
    }
  },
  methods: {
    check() {
      fetch("https://amersulaimandbc.pythonanywhere.com/check_domain/" + this.domain)
        .then(response => response.json())
        .then((data)=>{
          if (data.result === 'not blacklisted'){
            this.is_blacklisted = false
          } else if (data.result === 'blacklisted') {
            this.is_blacklisted = true;
          }
        });
        this.submitted_once = true
    }
  }
}
</script>

<style scoped>
.home {
  margin: 0 auto
}
</style>
