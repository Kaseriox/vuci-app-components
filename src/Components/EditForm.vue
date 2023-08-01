<template>
    <vuci-form uci-config="vuci_components_task" @applied="applied">
        <vuci-named-section :name="this.name" v-slot="{ s }" >
          <vuci-form-item-select :uci-section="s" :label="'Protocol'" name="proto" :options="ProtoOptions"/>
          <vuci-form-item-input :uci-section="s" :label="'IP address'" name="address" depend="proto === 'static'" rules="ip4addr" />
          <vuci-form-item-input :uci-section="s" :label="'Netmask'" name="netmask" depend="proto === 'static'" rules="netmask4" />
          <vuci-form-item-input :uci-section="s" :label="'Gateway'" name="gateway" depend="proto === 'static'" rules="ip4addr" />
          <vuci-form-item-list :uci-section="s" :label="'DNS'" name="dns" rules="ip4addr" depend="proto === 'static'"/>
        </vuci-named-section >
      </vuci-form>
</template>
<script>
export default {
  props: {
    name: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      ProtoOptions: ['static', 'dhpc']
    }
  },
  methods: {
    async applied () {
      await this.$uci.load('vuci_components_task')
      const data = await this.$uci.get('vuci_components_task', this.name)
      if (data && data.proto === 'dhpc') {
        for (const [key] of Object.entries(data)) {
          if (!key.includes('.') && !(key === 'name' || key === 'proto')) {
            await this.$uci.del('vuci_components_task', this.name, key)
          }
        }
        await this.$uci.save()
      }
      this.$emit('CloseModal')
    }
  }
}
</script>
