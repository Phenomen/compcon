<template>
  <cc-solo-dialog ref="dialog" icon="mdi-text-subject" large no-confirm title="Pilot Statblock">
    <v-card-text>
      <span>Convert to Roll20...</span>
      <v-select
        v-model="mechSelect"
        :items="pilot.Mechs"
        item-text="Name"
        item-value="ID"
        label="Select Mech"
        required
        outlined
      />
      <v-btn @click="doConvert">Convert</v-btn>
    </v-card-text>
  </cc-solo-dialog>
</template>

<script lang="ts">
import Vue from 'vue'
import roll20ToPilot from '@/io/Roll20'

export default Vue.extend({
  name: 'roll20-dialog',
  props: {
    pilot: {
      type: Object,
      required: true,
    },
  },
  data: () => ({
    mechSelect: '',
  }),
  methods: {
    show() {
      this.$refs.dialog.show()
    },
    hide() {
      this.$refs.dialog.hide()
    },
    async doConvert() {
      const mech = this.pilot.Mechs.find(mech => mech.ID === this.mechSelect)
      const converted = roll20ToPilot(this.pilot, mech)
      navigator.clipboard.writeText(JSON.stringify(converted, null, 4)).then(
        function() {
          Vue.prototype.$notify('Roll20 data copied to clipboard.', 'confirmation')
        },
        function() {
          Vue.prototype.$notifyError('Unable to copy Roll20 data')
        }
      )
    },
  },
})
</script>
