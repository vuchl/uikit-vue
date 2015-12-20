<template>
  <div class="uk-modal" v-el:modal>
    <div class="uk-modal-dialog" :class="classes">
      <slot></slot>
    </div>
  </div>
</template>

<script>
var UI = require('uikit')
var Vue = require('vue')

export default {
  data: function () {
    return {
      opened: false
    }
  },
  props: {
    large: Boolean,
    lightbox: Boolean,
    blank: Boolean,
    closed: Function,
    modifier: {
      type: String,
      default: ''
    },
    options: {
      type: Object,
      default: function () {
        return {}
      }
    }
  },
  ready: function () {
    var vm = this

    this.modal = UI.modal(this.$els.modal, Vue.util.extend({}, this.options))
    this.modal.on('hide.uk.modal', function () {

      vm.opened = false

      if (vm.closed) {
        vm.closed()
      }
    })

    this.modal.on('show.uk.modal', function () {

      // catch .uk-overflow-container
      setTimeout(function() {
        vm.modal.resize()
      }, 1)
    })

  },
  computed: {
    classes: function () {
      var classes = this.modifier.split(' ')

      if (this.large) {
        classes.push('uk-modal-dialog-large')
      }

      if (this.lightbox) {
        classes.push('uk-modal-dialog-lightbox')
      }

      if (this.blank) {
        classes.push('uk-modal-dialog-blank')
      }

      return classes
    }
  },
  methods: {
    open: function () {
      this.opened = true
      this.modal.show()
    },
    close: function () {
      this.modal.hide()
    }
  }
}
</script>
