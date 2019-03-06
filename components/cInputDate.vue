<!--
  Chamada:
    <c-input-date :value="data" :set="(v) => { data = v }" label="Data" />
-->
<template>
    <q-input stack-label lazy-rules bottom-slots ref="input"
      v-on="$listeners"
      v-bind="$attrs"
      v-model="inputModel"
      mask="##/##/####" hint="DD/MM/YYYY"
      :error="!isValid"
      error-message="data inválida"
    >
      <template v-slot:append>
        <q-icon name="event" class="cursor-pointer">
          <q-popup-proxy ref="popup">
            <q-date today-btn
              v-model="calendarModel"
              :default-year-month="defaultYearMonth"
            />
          </q-popup-proxy>
        </q-icon>
      </template>
    </q-input>
</template>

<style>
</style>

<script>
import { date } from 'quasar'

export default {
  name: 'cInputDate',
  props: ['value', 'set'],
  data () {
    return {
      isValid: true
    }
  },
  computed: {
    defaultYearMonth () {
      return date.formatDate(new Date(), 'YYYY/MM')
    },
    inputModel: {
      get: function () {
        return this.Model ? date.formatDate(new Date(this.Model), 'DD/MM/YYYY') : ''
      },
      set: function (value) {
        this.Model = null
        this.isValid = true
        if (value.length === 10) {
          let val = value.substr(6, 4) + '/' + value.substr(3, 2) + '/' + value.substr(0, 2)
          if ((new Date(val)).toString() === 'Invalid Date') {
            this.isValid = false
          } else {
            this.isValid = true
            console.debug(val)
            this.Model = val
          }
        }
      }
    },
    calendarModel: {
      get: function () {
        return this.Model || ''
      },
      set: function (value) {
        if (new Date(this.Model).getDate() !== new Date(value).getDate()) this.$refs.popup.hide()
        this.Model = value
        this.isValid = true
      }
    },
    Model: {
      get: function () {
        return this.value
      },
      set: function (value) {
        this.set(value)
      }
    }
  }
}
</script>
