<template>
  <div class="uk-modal" v-el:modal>
    <div class="uk-modal-dialog" :class="classes">
      <slot v-if="!content"></slot>
      <div v-if="content" class="uk-modal-content" v-html="content"></div>
      <div v-if="confirmation" class="uk-modal-footer uk-text-right">
        <button class="uk-button uk-button-small" @click="hide">Cancel</button>
        <button class="uk-button uk-button-small uk-button-primary" @click="confirmed">Ok</button>
      </div>
    </div>
  </div>
</template>

<script>
var UI = require('uikit')
var Vue = require('vue')

export default {
  data: function () {
    return {
      confirmation: false,
      confirmCallback: null
    }
  },
  props: {
    content: {
      type: String,
      default: '',
      twoWay: true
    },
    large: {
      type: Boolean,
      default: false
    },
    lightbox: {
      type: Boolean,
      default: false
    },
    blank: {
      type: Boolean,
      default: false
    },
    closed: {
      type: Function
    },
    modifier: {
      type: String,
      default: ''
    },
    options: {
      type: Object,
      default: function () {
        return {
          keyboard: true,
          bgclose: true,
          minScrollHeight: 150,
          center: false,
          modal: true
        }
      }
    }
  },
  ready: function () {
    var vm = this

    this.modal = UI.modal(this.$els.modal, Vue.util.extend({}, this.options))
    this.modal.on('hide.uk.modal', function () {
      // execute callback
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
    show: function () {
      this.confirmation = false
      this.modal.show()
      return this
    },
    hide: function () {
      this.confirmCallback = function () {}
      this.modal.hide()
      return this
    },
    block: function () {
      this.modal.options.bgclose = false
      this.modal.options.keyboard = false
      this.modal.options.modal = false
      return this
    },
    unblock: function () {
      this.modal.options.bgclose = true
      this.modal.options.keyboard = true
      this.modal.options.modal = true
      return this
    },
    confirm: function (msg, callback) {
      this.$set('content', msg)
      this.visible = true
      this.confirmation = true
      this.confirmCallback = callback
      this.modal.show()
    },
    confirmed: function () {
      if (this.confirmCallback) {
        this.confirmCallback()
      }
      this.confirmation = false
    }
  }
}
</script>
