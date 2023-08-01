<template>
  <div>
    <Table @OpenModal="OpenModal($event)" :key="TableKey" @Refresh="Refresh" />
    <CreateInterface @OpenModal="OpenModal($event)"/>
    <a-modal :title="`Interface ${label}`" v-model="EditModal" :destroyOnClose="true" :footer="null">
      <EditForm @CloseModal="CloseModal" :name="name"/>
    </a-modal>
  </div>
</template>

<script>
import Table from './src/Components/Table.vue'
import EditForm from './src/Components/EditForm.vue'
import CreateInterface from './src/Components/CreateInterface.vue'
export default {
  components: { Table, EditForm, CreateInterface },
  data () {
    return {
      EditModal: false,
      name: undefined,
      label: '',
      TableKey: 1
    }
  },
  methods: {
    OpenModal (event) {
      this.name = event.name
      this.label = event.label
      this.EditModal = true
    },
    CloseModal () {
      this.EditModal = false
      this.Refresh()
    },
    Refresh () {
      this.TableKey += 1
    }
  }
}
</script>
