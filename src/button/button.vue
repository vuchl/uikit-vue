<template>
	<button v-if="isbutton" class="uk-button" :class="classes">{{text}}</button>
	<a v-else class="uk-button" :class="classes">{{text}}</a>
</template>

<script>
export default {
  props: {
    // current page
    color: {
      type: String,
      default: ''
    },
    disabled: {
    	type: Boolean,
    	default: false
    },
    href: {
      type: String,
      default: '#'
    },
    isbutton: {
      type: Boolean,
      default: true
    },
    size: {
      type: String,
      default: ''
    },
    text: {
      type: String,
      default: 'Button'
    },
    width: {
      type: String,
      default: ''
    },
    
   },
   computed: {
   		classes: function () {
   			var classes = [];
   			if (this.color) {
   				switch (this.color) {
   					case 'primary':
   						classes.push('uk-button-primary');
   						break;
  					case 'success':
  						classes.push('uk-button-success');
  						break;
  					case 'danger':
  						classes.push('uk-button-danger');
  						break;
  					case 'link':
  						classes.push('uk-button-link');
  						break;
  					default: {}
   				}
   			}

   			if (this.size) {
   				switch (this.size) {
   					case 'mini':
   						classes.push('uk-button-mini');
   						break;
  					case 'small':
  						classes.push('uk-button-small');
  						break;
  					case 'large':
  						classes.push('uk-button-large');
  						break;
  					default: {}
   				}
   			}

   			if (this.width) {
          if(this.width == 'fullwidth') {
            classes.push('uk-width-1-1');
          }
          else
          {
   				 classes.push('uk-width-' + this.width); 
          }
   			}
        //this.$log(classes);
        return classes;
   		},
   }
}
</script>
