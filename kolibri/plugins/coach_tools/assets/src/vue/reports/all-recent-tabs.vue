<template>

  <div class="tabs">
    <router-link :to="recentViewLink">
      <div class="tab" :class="{ active: isRecentView }">{{$tr('recent')}}</div>
    </router-link>
    <router-link :to="allViewLink">
      <div class="tab" :class="{ active: !isRecentView }">{{$tr('topics')}}</div>
    </router-link>
  </div>

</template>


<script>

  module.exports = {
    $trNameSpace: 'allRecentTabs',
    $trs: {
      recent: 'Recent',
      topics: 'Topics',
    },
    props: {
      recentViewLink: {
        type: Object,
        required: true,
      },
      allViewLink: {
        type: Object,
        required: true,
      },
      isRecentView: {
        type: Boolean,
        required: true,
      },
    },
  };

</script>


<style lang="stylus" scoped>

  @require '~kolibri.styles.coreTheme'

  .tabs
    margin-top: 0.5em
    margin-bottom: 0.5em
    background-color: $core-bg-light

  a
    display: inline-block
    text-decoration: none


  .tab
    display: inline-block
    padding: 1em
    color: $core-text-default
    background-color: $core-bg-light
    border-bottom: solid 3px $core-bg-light

  .active
    border-bottom: solid 3px $core-action-normal

</style>
