<template>
  <div>
    <v-btn color="#22BB66" large dark class="btn" @click="isShow = true">CREATE</v-btn>
    <v-row justify="center">
      <v-dialog
        v-model="isShow"
        persistent
        max-width="600px"
      >
        <v-card>
          <v-card-title>
            <span class="text-h5">Create</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="6" >
                  <v-text-field
                    v-model="name"
                    label="Name"
                    outlined
                    hide-details
                  />
                </v-col>
                <v-col cols="3" >
                  <v-text-field
                    v-model="post_code"
                    label="Post Code"
                    outlined
                    hide-details
                  />
                </v-col>
                <v-col cols="3" >
                  <v-text-field
                    v-model="price"
                    label="Price"
                    outlined
                    hide-details
                  />
              </v-col>
              <v-col cols="12">
                <v-textarea
                  v-model="desc"
                  solo
                  name="input-7-4"
                  label="Description"
                  hide-details
                />
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions class="justify-center">
          <v-btn color="white" large class="group-btn" @click="isShow = false">CANCEL</v-btn>
          <v-btn color="#22BB66" large dark class="group-btn" @click="submit">CREATE</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
  </div>
</template>

<script>
import Swal from "sweetalert2";
import axios from 'axios'

  export default {
    Props: {
      show: Boolean,
    },
    data() {
      return{
        isShow: false,
        name: '',
        post_code: '',
        price: '',
        desc: '',
      }
    },
    methods: {
      submit() {
        try {
          axios.post('https://test-backend.baania.dev/home/', {
            name: this.name,
            post_code: this.post_code,
            price: this.price,
            desc: this.desc
          }).then(response => {
            Swal.fire({
              icon: 'success',
              title: 'Success',
              text: 'Create a Successful!',
              confirmButtonText: 'CONTINUE',
              confirmButtonColor: '#C0C5CE',
            }).then((result) => {
              if (result.isConfirmed) {
                this.isShow = false
                this.refresh()
              }
            })
          })
        } catch (error) {
          Swal.fire({
            icon: 'error',
            title: 'Fail',
            text: 'Let’s try one more again',
            confirmButtonText: 'TRY AGAIN',
            confirmButtonColor: '#C0C5CE',
          })
        }
      },
      refresh() {
        this.$nuxt.refresh()
      }
    },
  }
</script>

<style scoped>
.btn {
  width: 200px;
  height: 53px;
}
.group-btn {
  width: 164px;
  height: 45px;
}
</style>