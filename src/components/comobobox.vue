<template>
    <div class="combobox-container" v-on:keyup.40="arrowDown()" v-on:keyup.38="arrowUp()" v-on:keyup.13="enter()" @click="focus()">
        <input type="text" :class="isFocus ? 'selected border-forcus' : 'selected'" v-model="keySelected" v-on:keyup="autocomplete()" ref="inputcombobox">
        <div class="select" v-if="isActived">
            <div class="option" v-for="(item,index) in itemsInFilter" :key="index" :class="index == indexSelected ? 'isselected' : ''" @click="enter(index)">
              <div class="selected-icon" v-if="itemsInFilter[index] === itemSelected"></div>
              {{item.text}}
              </div>
        </div>
        <div :class="isFocus ? 'dropdow-button border-forcus ' : 'dropdow-button'" @click="onclickComboboxButton()" ><span class = "dropdown-icon" :class="isActived ? 'rotate' : ''"></span></div>
    </div>
</template>
<script>
export default {
  name: 'mscombobox',
  data () {
    return {
      items: [{ value: 0, text: 'Nam' }, { value: 1, text: 'Nữ' }, { value: 2, text: 'Khác' }, { value: 3, text: 'Khác1' }, { value: 4, text: 'Khác2' }],
      itemSelected: { value: -1, text: '' },
      isActived: false,
      keySelected: '',
      indexHover: 0,
      keyfilter: '',
      isFocus: false,
      indexSelected: 0
    }
  },
  methods: {
    onclickComboboxButton () {
      this.$refs.inputcombobox.focus()
      this.keyfilter = ''
      if (this.isActived) { this.isActived = false } else this.isActived = true
    },
    autocomplete () {
      if (this.keySelected === '' || this.keySelected === this.itemSelected.text) return
      if (this.itemsInFilter.length > 0) {
        this.isActived = true
      }
    },
    arrowDown () {
      if (this.indexHover < this.itemsInFilter.length - 1) { this.indexHover++ }
    },
    arrowUp () {
      if (this.indexHover > 0) { this.indexHover-- }
    },
    enter (index) {
      if (index != null) {
        this.itemSelected = this.itemsInFilter[index]
        this.keySelected = this.itemsInFilter[index].text
        this.indexHover = index
        this.isActived = false
        this.setIndexSelected()
      } else {
        if (this.itemsInFilter[this.indexHover] != null) {
          this.itemSelected = this.itemsInFilter[this.indexHover]
          this.keySelected = this.itemsInFilter[this.indexHover].text
          this.isActived = false
          this.setIndexSelected()
        }
      }
    },
    setIndexSelected () {
      for (var i = 0; i < this.items.length; i++) {
        if (this.items[i] === this.itemSelected) {
          this.indexSelected = i
          return
        }
      }
    },
    getValue () {
      return this.itemSelected.value
    },
    getText () {
      return this.itemSelected.text
    },
    focus () {
      this.isFocus = true
    }
  },
  computed: {
    itemsInFilter () {
      return this.items.filter(item => item.text.toLowerCase().includes(this.keyfilter.toLowerCase()))
    }
  },
  watch: {
    keySelected: function () {
      this.keyfilter = this.keySelected
      if (this.itemsInFilter.length === 0) this.isActived = false
      if (this.keySelected === '') this.isActived = false
      if (this.keySelected !== this.itemSelected.text) { this.itemSelected = { value: -1, text: '' }; this.indexSelected = 0; this.indexHover = 0 }
    },
    indexHover: function () {
      this.indexSelected = this.indexHover
    }
  }
}
</script>
