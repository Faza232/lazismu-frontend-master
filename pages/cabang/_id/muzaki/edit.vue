<template>
    <section class="flex flex-col items-center justify-center px-4">
      <div class="text-[32px] font-semibold text-dark">Edit Data</div>
      <p class="mt-4 text-base leading-7 text-center mb-[50px] text-grey">
        <i>Muzakki</i> <br />
      </p>
      <form class="w-full card" @submit="update">
        <p v-if="listerror" class="m-3">
          <b>Tolong Di Cek Kembali :</b>
          <ul class="list-disc">
            <li v-for="error in errors" :key="error" class="text-red-500">
            {{ error[0] }}
            </li>
          </ul>
          </p>
          <div class="form-group">
        <label for="" class="text-grey">Nama</label>
        <input
          type="text"
          class="input-field"
          value="muzaki.nama"
          v-model="muzaki.nama"
          
        />
      </div>
      <div class="form-group">
        <label for="" class="text-grey">NIK</label>
        <input
          type="text"
          class="input-field"
          value="muzaki.nik"
          v-model="muzaki.nik"
          
        />
      </div>
      <div class="form-group">
        <label for="" class="text-grey">Alamat</label>
        <textarea
          class="input-field"
          v-model="muzaki.alamat"
          
        ></textarea>
      </div>
      <div class="form-group">
  <label for="" class="text-grey">Nomor Telepon</label>
  <input
    type="number"
    id="phone"
    class="input-field"
    v-model="muzaki.noTelp"
    
/>
</div>
      <div class="form-group">
        <label for="" class="text-grey">NPWP</label>
        <input
          type="number"
          class="input-field"
          value="muzaki.npwp"
          v-model="muzaki.npwp"
          
        />
      </div>
        <button type="submit" class="w-full btn btn-lazismu mt-[14px]">
          Update
        </button>
      </form>
    </section>
  </template>
  
  <script>
  export default {
    layout: 'form',
    middleware: 'auth',
    data() {
      return {
        muzaki: {
          nama: '',
          nik: '',
          alamat: '',
          noTelp: '',
          npwp: '',
        },
        listerror : false,
        errors: [],
        cabang_id: JSON.parse(localStorage.getItem('cabang_id')),
        id: this.$route.params.item,
      }
    },
    watch: {
      errors(){
        this.listerror = true
      }
    },
    async fetch() {
      await this.$axios
        .get('/muzaki', {
          params: {
            limit: 1,
            cabang_id: this.muzaki.cabang_id,
            id: this.id,
          },
        })
        .then((response) => {
          ;(this.muzaki.nama = response.data.result.nama),
            (this.muzaki.nik = response.data.result.nik),
            (this.muzaki.alamat = response.data.result.alamat),
            (this.muzaki.noTelp = response.data.result.noTelp),
            (this.muzaki.npwp = response.data.result.npwp)
        })
    },
    methods: {
      async update(e) {
        e.preventDefault()
        //send data ke Rest API untuk update
        await this.$axios
          .post(`/muzaki/update/${this.$route.params.item}`, {
            //data yang dikirim
            nama: this.muzaki.nama,
            nik: this.muzaki.nik,
            cabang_id: this.cabang_id,
            alamat: this.muzaki.alamat,
            noTelp: this.muzaki.noTelp,
            npwp: this.muzaki.npwp,
          })
          .then(() => {
            //redirect ke route "post"
            this.$router.push({
              name: 'cabang-id-muzaki',
              params: {
              id: this.muzaki.cabang_id,
            },
            })
          })
          .catch((error) => {
            //assign error validasi
            this.errors = error.response.data.errors
            
          })
      },
    },
  }
  </script>
  