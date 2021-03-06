<template>

  <div class="dropdown">
    <button
      ref="dropdownbutton"
      class="dropdown-button"
      @click="toggleDropdown"
      aria-haspopup="true">
      <span class="dropdown-button-text" :class="{uparrow: dropdownopen}">
        {{ $tr('downloadContent') }}
      </span>
    </button>
    <ul
      ref="dropdownitems"
      class="dropdown-items"
      :class="{ dropdownopen: dropdownopen }"
      :aria-hidden="dropDownOpenText"
      role="menu">
      <li
        v-for="file in files"
        class="dropdown-item"
        role="presentation">
        <a
          download
          class="dropdown-item-link"
          @click="toggleDropdown"
          :href="file.download_url"
          role="menuitem">
          {{ file.preset + ' (' + prettifyFileSize(file.file_size) + ')' }}
        </a>
      </li>
    </ul>
  </div>

</template>


<script>

  const filesize = require('filesize');

  module.exports = {
    components: {
      'icon-button': require('kolibri.coreVue.components.iconButton'),
    },
    $trNameSpace: 'downloadButton',
    $trs: {
      downloadContent: 'Download Content',
    },
    props: {
      files: {
        type: Array,
        default: () => [],
      },
    },
    data() {
      return {
        dropdownopen: false,
        itemSelected: 0,
      };
    },
    computed: {
      dropDownOpenText() {
        return String(this.dropdownopen);
      },
      dropDownItems() {
        let listItems = this.$refs.dropdownitems.children;
        listItems = [...listItems];
        const anchorItems = [];
        listItems.forEach((li) => {
          anchorItems.push(li.children[0]);
        });
        return anchorItems;
      },
      focusableItems() {
        let focusableItems = [];
        focusableItems.push(this.$refs.dropdownbutton);
        focusableItems = focusableItems.concat(this.dropDownItems);
        return focusableItems;
      },
    },
    methods: {
      prettifyFileSize(bytes) {
        return filesize(bytes);
      },
      toggleDropdown() {
        this.dropdownopen = !this.dropdownopen;
        this.itemSelected = 0;
      },
      handleKeys(e) {
        if (this.dropdownopen) {
          switch (e.keyCode) {
            case 40: // down
              e.stopPropagation();
              e.preventDefault();
              this.itemSelected++;
              this.focusOnItem();
              return;

            case 38: // up
              e.stopPropagation();
              e.preventDefault();
              this.itemSelected--;
              this.focusOnItem();
              return;

            case 9: // tab
              e.stopPropagation();
              e.preventDefault();
              if (this.itemSelected === (this.focusableItems.length - 1)) {
                this.toggleDropdown();
                return;
              }
              this.itemSelected++;
              this.focusOnItem();
              return;

            case 27: // esc
              e.stopPropagation();
              e.preventDefault();
              this.toggleDropdown();
              return;

            default:
              return;
          }
        }
      },
      focusOnItem() {
        this.itemSelected =
          Math.min(Math.max(this.itemSelected, 0), (this.focusableItems.length - 1));
        this.focusableItems[this.itemSelected].focus();
      },
    },
    mounted() {
      document.addEventListener('keydown', this.handleKeys);
    },
    beforeDestroy() {
      document.removeEventListener('keydown', this.handleKeys);
    },
  };

</script>


<style lang="stylus" scoped>

  @require '~kolibri.styles.coreTheme'

  .dropdown
    display: inline-block
    position: relative

  .dropdown-button
    padding: 0.5em
    margin-top: 1em
    margin-bottom: 1em
    font-size: smaller

  .dropdown-button-text
    &:after
      padding-left: 0.5em
      content: '\25BC'

  .uparrow
    &:after
      content: '\25b2'

  .dropdown-items
    background-color: white
    list-style: none
    padding: 0
    margin: 0
    margin-top: -0.8em
    display: none
    position: absolute

  .dropdown-item
    padding: 0
    margin: 0
    width: 100%
    position: relative
    display: block

  .dropdown-item-link
    padding: 0.5em
    margin: 0
    width: 100%
    display: block
    text-decoration: none
    white-space: nowrap
    font-size: smaller
    &:focus
      background-color: $core-action-light
    &:hover
      background-color: $core-action-light
      outline: $core-action-light 2px solid

  .dropdownopen
    display: block

</style>
