<template>
  <div>
    <div class="bg w-auto h-32 ">
      <v-container>
        <v-row no-gutters class="mt-10">
          <v-col cols="4">
            <p>URL</p>
          </v-col>
          <v-col cols="1"></v-col>
          <v-col cols="4">
            <p>PORT</p>
          </v-col>
          <v-col cols="4">
            <v-text-field
            placeholder="http://localhost"
            outlined
            solo
            dense
          />
          </v-col>
          <v-col cols="1"></v-col>
          <v-col cols="4">
            <v-text-field
            placeholder="8000"
            outlined
            solo
            dense
          />
          </v-col>
          <v-col cols="1"></v-col>
          <v-col cols="1" class="text-center">
            <v-btn color="#3C64B1" large dark class="btn">CONNECT</v-btn>
          </v-col>
        </v-row>
      </v-container>
    </div>
    <v-container>
      <v-row>
        <v-col cols="6">HOUSE LIST</v-col>
        <v-col cols="6" class="text-right">
          <DialogCreate/>
        </v-col>
      </v-row>
      <v-data-table
        :headers="headers"
        :items="lists"
        class="elevation-1 mt-10"
      >
        <template v-slot:[`item.actions`]="{ item }">
          <v-btn
            elevation="0"
            color="#FFF7E6"
            rounded
            class="action mr-2"
            @click="editItem(item)"
          >
            <span style="color: #FF9900;">VIEW DETAIL</span>
          </v-btn>
          <v-btn
            elevation="0"
            color="#FDF4F7"
            rounded
            class="action mr-2"
            @click="deleteItem(item)"
          >
            <span style="color: #B93E5C;">DELETE</span>
          </v-btn>
        </template>
        <template v-slot:[`no-data`]>
          <v-btn
            color="primary"
            @click="initialize"
          >
            Reset
          </v-btn>
        </template>
      </v-data-table>
      <div class="bg w-auto h-32 mt-10 py-8">
        <v-container class="justify-center">
          <v-row>
            <v-col cols="4"></v-col>
            <v-col cols="4">
              <v-autocomplete
                v-model="seleted"
                elevation="0"
                solo
                placeholder="SELECT POST CODE"
                :items="items"
                item-text="post_code"
                item-value="post_code"
                style="width: 405px; height: 53px;"
              />
            </v-col>
            <v-col cols="4"></v-col>
            <v-col cols="4"></v-col>
            <v-col v-if="seleted" cols="6" class="">
              <div style="color: #3C64B1;">
                <p>Average : 1000000.000000000000</p>
                <p>Median : 1000000.0</p>
              </div>
            </v-col>
          </v-row>
        </v-container>
      </div>
    </v-container>
    <DialogView :show="isShow" :item="itemEdit" @close="close"/>
  </div>
</template>

<script>
import Swal from "sweetalert2";
import axios from 'axios'
import DialogCreate from '../components/DialogCreate' 
import DialogView from '../components/DialogView' 

export default {
  name: 'IndexPage',
  data() {
    return {
      components: {
        DialogCreate,
        DialogView,
      },
      isShow: false,
      itemEdit: null,
      seleted: '',
      headers: [
        {
          text: 'ID',
          sortable: false,
          value: 'id',
          align: 'center'
        },
        { text: 'Name', value: 'name', align: 'center' },
        { text: 'Post Code', value: 'post_code', align: 'center' },
        { text: 'Price', value: 'price', align: 'center' },
        { text: 'Actions', value: 'actions', sortable: false, align: 'center' },
      ],
      lists: [],
      items:[]
    }
  },
  async fetch() {
    try {
      const { payload: data } = await fetch(
      'https://test-backend.baania.dev/home?skip=1&take=100'
    ).then(res => res.json())
    this.lists = data

    const { payload: postCode } = await fetch(
      'https://test-backend.baania.dev/postCode'
    ).then(res => res.json())
    this.items = postCode
    } catch (error) {
      console.log('AAAA', error)
    }
  },
  methods: {
    create(){
    },
    editItem(item){
      this.isShow = true
      this.itemEdit = item
    },
    deleteItem(item){
      try {
          axios.delete('https://test-backend.baania.dev/home/' + `${ item.id }`, {
            item
          }).then(response => {
            Swal.fire({
              icon: 'success',
              title: 'Success',
              text: 'Delete a Successful!',
              confirmButtonText: 'CONTINUE',
              confirmButtonColor: '#C0C5CE',
            }).then((result) => {
              this.refresh()
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
      this.isShow = false
    },
    refresh() {
      this.$nuxt.refresh()
    },
    async initialize(){
      try {
        const { payload: data } = await fetch(
          'https://test-backend.baania.dev/home?skip=1&take=100'
        ).then(res => res.json())
        this.lists = data
      } catch (error) {
        console.log('AAAA', error)
      }
    },
  },
}
</script>

<style scoped>
.bg{
  background-color: #F4F7FC;
}
.btn {
  width: 200px;
  height: 53px;
}
.action{
  width: 130px;
  height: 38px;
  font-size: 14px;
}
</style>