<template>
      <v-dialog
        v-model="isShow"
        persistent
        max-width="600px"
      >
        <v-card>
          <v-card-title>
            <span class="text-h5">Item Detail - {{ itemEdit.id }}</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="6" >
                  <v-text-field
                    v-model="itemEdit.name"
                    label="Name"
                    outlined
                    :rules="rules"
                    required
                  />
                </v-col>
                <v-col cols="3" >
                  <v-text-field
                    v-model="itemEdit.post_code"
                    label="Post Code"
                    outlined
                    :rules="rules"
                    required
                  />
                </v-col>
                <v-col cols="3" >
                  <v-text-field
                    v-model="itemEdit.price"
                    label="Price"
                    outlined
                    :rules="rules"
                    required
                  />
              </v-col>
              <v-col cols="12">
                <v-textarea
                  v-model="itemEdit.desc"
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
          <v-btn color="white" large class="group-btn" @click="close">CANCEL</v-btn>
          <v-btn color="#F6A623" large dark class="group-btn" @click="submit">UPDATE</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
</template>

<script>
import Swal from "sweetalert2";
import axios from 'axios'

  export default {
    props: {
      show: {
        type: Boolean,
        default: false,
      },
      item: {
        type: Object,
        default: null,
      },
    },
    data() {
      return{
        isShow: false,
        itemEdit: {},
        rules: [
          val => (val || '').length > 0 || 'This field is required'
        ],
      }
    },
    watch:{
      item: {
        handler(value) {
          this.itemEdit = value
        },
      },
      show: {
        handler(value) {
          this.isShow = value
        },
      }
    },
    methods: {
      submit() {
        try {
          axios.patch('https://test-backend.baania.dev/home/' + `${ this.itemEdit.id }`, {
            name: this.itemEdit.name,
            post_code: this.itemEdit.post_code,
            price: this.itemEdit.price,
            desc: this.itemEdit.desc
          }).then(response => {
            Swal.fire({
              icon: 'success',
              title: 'Success',
              text: 'Update a Successful!',
              confirmButtonText: 'CONTINUE',
              confirmButtonColor: '#C0C5CE',
            }).then((result) => {
              if (result.isConfirmed) {
                this.$emit('close')
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
      close(){
        this.$emit('close')
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