<template>
  <div class="uk-flex uk-flex-middle" :class="collaboratorClass">
    <avatar-image
      v-if="isUser"
      class="oc-mr-s"
      :width="48"
      :userid="item.value.shareWith"
      :user-name="item.label"
    />
    <template v-else>
      <oc-icon
        v-if="item.value.shareType === shareTypes.group"
        key="avatar-group"
        class="oc-mr-s"
        name="group"
        size="xlarge"
      />
      <oc-icon v-else key="avatar-generic-person" class="oc-mr-s" name="person" size="xlarge" />
    </template>
    <div class="files-collaborators-autocomplete-user-text">
      <div class="oc-text-bold files-collaborators-autocomplete-username" v-text="item.label" />
      <div v-if="item.value.shareWithAdditionalInfo" v-text="item.value.shareWithAdditionalInfo" />
    </div>
  </div>
</template>

<script>
import Mixins from '../../mixins/collaborators'
import { shareTypes } from '../../helpers/shareTypes'

export default {
  name: 'AutocompleteItem',

  mixins: [Mixins],

  props: {
    item: {
      type: Object,
      required: true
    }
  },

  data() {
    return {
      shareTypes,
      loading: false
    }
  },

  computed: {
    isUser() {
      return this.item.value.shareType === shareTypes.user
    },

    isRemoteUser() {
      return this.item.value.shareType === shareTypes.remote
    },

    collaboratorClass() {
      const isUser = this.isUser || this.isRemoteUser

      return (
        'files-collaborators-search-' + (isUser ? (this.isRemoteUser ? 'remote' : 'user') : 'group')
      )
    }
  }
}
</script>
