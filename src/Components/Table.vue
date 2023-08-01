<template>
  <div>
    <vuci-form uci-config="vuci_components_task">
      <vuci-typed-section type="interface" :columns="columns">
        <template #name="{ s }">
          <vuci-form-item-dummy :uci-section="s" name="name" />
        </template>
        <template #address="{ s }">
          <vuci-form-item-dummy :uci-section="s" name="address" />
        </template>
        <template #netmask="{ s }">
          <vuci-form-item-dummy :uci-section="s" name="netmask" />
        </template>
        <template #actions="{ s }">
          <a-space>
            <a-button type="primary" @click="handleEdit(s)">Edit</a-button>
            <a-popconfirm
              :title="`Are You Sure You Want To Delete ${s['name']} Interface?`"
              ok-text="Yes"
              cancel-text="No"
              @confirm="DeleteInterface(s)"
              >
              <a-button type="danger">Delete</a-button>
            </a-popconfirm>
          </a-space>
        </template>
      </vuci-typed-section>
      <template #footer>
        <div></div>
      </template>
    </vuci-form>
  </div>

</template>
<script>
export default {
  data () {
    return {
      columns: [
        { name: 'name', label: 'Interface Name' },
        { name: 'address', label: 'IP address' },
        { name: 'netmask', label: 'Netmask' },
        { name: 'actions' }
      ],
      Inputs: [
        { name: 'proto' },
        { name: 'address' },
        { name: 'netmask' },
        { name: 'gateway' },
        { name: 'dns' }
      ]
    }
  },
  methods: {
    async handleEdit (s) {
      this.$emit('OpenModal', { label: s.name, name: s['.name'] })
    },
    async DeleteInterface (s) {
      await this.$uci.del('vuci_components_task', s['.name'])
      await this.$uci.save()
      this.$emit('Refresh')
    }
  }
}
</script>
