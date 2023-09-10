<template>
  <section class="flex flex-col items-center justify-center px-4">
    <div class="text-[32px] font-semibold text-dark">Tambah Data</div>
    <p class="mt-4 text-base leading-7 text-center mb-[50px] text-grey">
      Coa Kredit<i>(Chart of Account)</i> <br />
    </p>
    <form class="w-full card" @submit.prevent="createCoa">
      <p v-if="listerror" class="m-3">
        <b>Tolong Di Cek Kembali :</b>
        <ul class="list-disc">
          <li v-for="error in errors" :key="error[0]" class="text-red-500">
          {{ error[0] }}
          </li>
        </ul>
      </p>
      <!-- <button @click="fetch">Ambil Data</button>
      <table>
        <tr>
          <th>Nama</th>
          <th>Alamat</th>
        </tr>
        <tr for="item in data" :key="data">
          <td>{{item.nama}}</td>
          <td>{{item.alamat}}</td>
        </tr>
      </table> -->
      <div class="form-group">
        <label for="" class="text-grey">Nomer Urut</label>
        <input
          type="text"
          class="input-field"
          v-model="coa.kode"
          value="coa.kode"
          required
        />
      </div>

      <div class="form-group">
        <label for="" class="text-grey">Nama Akun</label>
          <input
            type="text"
            class="input-field"
            v-model="coa.name"
            required
          />
      </div>

      <div class="form-group">
        <label for="" class="text-grey">Tipe</label>
        <select
          name="tipe"
          id=""
          v-model="coa.tipe"
          class="input-field rounded-lg block p-2.5 text-sm"
          
        >
          <option value="0">Debit</option>
          <option value="1">Kredit</option>
        </select>
      </div>
      
      <div class="form-group">
        <label for="" class="text-grey">Front Office</label>
        <select
          name="isShow"
          id=""
          v-model="coa.isShow"
          class="input-field rounded-lg block p-2.5 text-sm"
          
        >
          <option value="0">Tidak</option>
          <option value="1">Ya</option>
        </select>
      </div>
      <button type="submit" class="w-full btn btn-lazismu mt-[14px]">
        Create
      </button>
    </form>
  </section>
</template>

<script>
export default {
  layout: 'form',
  // middleware: 'auth',
  data() {
    return {
      coa: {
        name: '',
        kode: '',
        cabang_id: this.$route.params.id,
        tipe: '',
        isShow: '',
      },
      name: '',
      isFocus: false,
      listerror: false,
      errors: [],
      data: [],
    }
  },

  mounted() {
    console.log(this.$route.params.id)
  },

  methods: {

    async createCoa() {
      try {
        //send registration data to server
        let response = await this.$axios.post('/coa', this.coa)

        //Redirect to my coa page
        this.$router.push({
          name: 'cabang-id-coa',
          params: {
            id: this.coa.cabang_id,
          },
        })

        console.log(response)
      } catch (error) {
        console.log(error)
        this.errors = error.response.data.errors
        this.listerror = true
      }
    },
  }
}
</script>
