<template lang="pug">
.ni-session-wrapper
  img.ni-session-backdrop(src="~assets/images/cosmos-logo.png")
  session-loading(v-if="config.modals.session.state == 'loading'")
  session-welcome(v-if="config.modals.session.state == 'welcome'")
  session-sign-up(v-if="config.modals.session.state == 'sign-up'")
  session-sign-in(v-if="config.modals.session.state == 'sign-in'")
  session-account-delete(v-if="config.modals.session.state == 'delete'")
  session-hardware(v-if="config.modals.session.state == 'hardware'")
  session-import(v-if="config.modals.session.state == 'import'")
</template>

<script>
import { mapGetters } from 'vuex'
import SessionLoading from 'common/NiSessionLoading'
import SessionWelcome from 'common/NiSessionWelcome'
import SessionSignUp from 'common/NiSessionSignUp'
import SessionSignIn from 'common/NiSessionSignIn'
import SessionHardware from 'common/NiSessionHardware'
import SessionImport from 'common/NiSessionImport'
import SessionAccountDelete from 'common/NiSessionAccountDelete'
export default {
  name: 'ni-session',
  components: {
    SessionLoading,
    SessionWelcome,
    SessionSignUp,
    SessionSignIn,
    SessionHardware,
    SessionImport,
    SessionAccountDelete
  },
  computed: { ...mapGetters(['config']) }
}
</script>

<style lang="stylus">
@import '~variables'

.ni-session-wrapper
  position relative
  z-index z(modal)

  .ni-session-backdrop
    position absolute
    top -10vw
    left -10vw
    width 50vw
    opacity 0.25

.ni-field-checkbox
  display flex
  flex-flow row nowrap
  align-items center

  .ni-field-checkbox-input
    flex 0 0 2rem
    height 2rem
    display flex
    align-items center
    justify-content center
    background app-fg
    input
      width auto
      display block
      padding 0
      margin 0

  .ni-field-checkbox-label
    flex 1
    line-height 1.375
    padding 0.5rem 1rem
    font-size 0.875rem

.ni-session
  position fixed
  top 0
  left 0
  z-index z(default)
  background app-bg

.ni-session-container
  &:not(.ni-form)
    width 100vw
    height 100vh
    display flex
    flex-flow column nowrap

  &.ni-form
    .ni-form-main
      width 100vw
      height 100vh
      display flex
      flex-flow column nowrap

.ni-session-header
  display flex
  flex-flow row nowrap
  justify-content space-between
  align-items center
  flex 0 0 3rem
  margin-top 1.5rem // for macos traffic signals

  a
    width 3rem
    display flex
    align-items center
    justify-content center
    cursor pointer
    height 3rem
    i
      color txt
      font-size lg
    &:hover
      background app-fg
      i
        color bright

  .ni-session-title
    flex 1
    padding 0 1rem
    font-size lg
    text-align center
    color bright

.ni-session-main
  flex 1
  display flex
  flex-flow column
  justify-content center
  min-height 0

  overflow-y auto

  .ps-scrollbar-y-rail
    display none

  > p
    padding 1rem
    border-bottom px solid bc

.ni-session-footer
  border-top 2*px solid bc-dim
  flex 0 0 5rem + px
  display flex
  align-items center
  justify-content center

  &:empty
    display none

.ni-session-footer > div
  display flex
  justify-content space-between

@media screen and (min-width: 768px)
  .ni-session-wrapper
    position fixed
    top 0
    left 0
    background darken(app-bg, 25%)
    width 100vw
    height 100vh

    display flex
    align-items center
    justify-content center

  .ni-session
    position static
    shadow()

  .ni-session-container
    &:not(.ni-form)
    &.ni-form .ni-form-main
      width 32rem
      min-height 28rem
      max-height 90vh
      height auto

  .ni-session-header
    background app-fg
    margin-top 0

  .ni-session-main
    padding 2rem

    .ni-form-group
      display block !important
</style>
