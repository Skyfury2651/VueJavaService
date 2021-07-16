<template>
  <div>
    <Table
        @show="show"
        :columns="columns"
        :data="students"
        @handleDelete="handleDelete"
    />
    <button id="show-modal" @click="showModal = true">Show Modal</button>
      <!-- use the modal component, pass in the prop -->
      <modal v-if="showModal" @close="showModal = false">
        <h3 slot="header">Header</h3>
        <div slot="body">
          <label>insert</label>
          <input type="name">
        </div>
      </modal>
  </div>
</template>

<script>
import Table from '../components/Table'
import Modal from "../components/Modal";
import axios from 'axios'

export default {
  name: "List",
  components: {
    Table,
    Modal
  },
  data() {
    return {
      columns: [
        {
          'label': 'Id',
          'field': 'id',
        }
      ],
      students: [
        {
          id: 1,
        },
        {
          id: 2
        }
      ],
      showModal: false
    }
  },
  methods: {
    async show(id = null) {
      this.isEdit = false
      if (id != null) {
        this.isEdit = true
        await this.asyncShow(id)
      }
      this.isShow = true
    },
    async save({data}) {
      await this.loadData()
      this.isShow = false
    },
    async handleDelete(id) {
      await this.loadData()
      this.isShow = false
    },
  },
  mounted() {
    axios.get('https://api.coindesk.com/v1/bpi/currentprice.json')
      .then(response => (this.info = response))
  }
}
</script>

<style scoped>

</style>