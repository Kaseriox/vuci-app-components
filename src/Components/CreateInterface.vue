<template>
    <div>
      <a-form layout="inline" :form="form" @submit.prevent="Create">
          <a-form-item label="Interface Name">
            <a-input placeholder="Input Interface Name" size="large"
            v-decorator="['Name', { rules: [{ required: true, message: 'Please input your interface name!' }] }]"
            />
          </a-form-item>
          <a-form-item>
            <a-button type="primary" size="large" html-type="submit">Create Interface</a-button>
          </a-form-item>
      </a-form>

    </div>
</template>
<script>
export default {
  data () {
    return {
      name: '',
      form: this.$form.createForm(this, { name: 'CreateForm' })
    }
  },
  methods: {
    async Create (e) {
      if (!this.Validate()) {
        return
      }
      const sid = await this.$uci.add('vuci_components_task', 'interface')
      this.$uci.set('vuci_components_task', sid, 'name', this.name)
      this.$emit('OpenModal', { label: this.name, name: sid })
      this.name = ''
    },
    Validate () {
      this.form.validateFields((err, values) => {
        if (!err) {
          this.name = values.Name
        }
      })
      if (this.name !== '') {
        return true
      }
    }
  }
}
</script>
