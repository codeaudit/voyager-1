<template lang='pug'>
.li-delegate(:class='styles'): .li-delegate__values
  .li-delegate__value.name
    router-link(:to="{ name: 'delegate', params: { delegate: delegate.id }}") {{ delegate.moniker }}
  .li-delegate__value.percent_of_vote
    span {{ num.percentInt(bondedPercent) }}
  .li-delegate__value.number_of_votes.num.bar
    span {{ num.prettyInt(delegate.voting_power) }}
    .bar(:style='vpStyles')
  .li-delegate__value.your-votes
    span {{ yourVotes }}
  .li-delegate__value.status
    span {{ delegateType }}
  template(v-if="userCanDelegate")
    .li-delegate__value.checkbox(v-if="yourVotes > 0")
      i.material-icons lock
    .li-delegate__value.checkbox#remove-from-cart(v-else-if="inCart" @click='rm(delegate)')
      i.material-icons check_box
    .li-delegate__value.checkbox#add-to-cart(v-else @click='add(delegate)')
      i.material-icons check_box_outline_blank
  template(v-else)
    .li-delegate__value
</template>

<script>
import { mapGetters } from 'vuex'
import num from 'scripts/num'
import Btn from '@nylira/vue-button'
import { maxBy } from 'lodash'
export default {
  name: 'li-delegate',
  props: ['delegate'],
  components: { Btn },
  computed: {
    ...mapGetters(['shoppingCart', 'delegates', 'config', 'committedDelegations', 'user']),
    yourVotes () {
      let yourVotes = this.num.prettyInt(this.committedDelegations[this.delegate.id])
      this.delegate.your_votes = yourVotes

      return yourVotes
    },
    styles () {
      let value = ''
      if (this.inCart || this.yourVotes > 0) value += 'li-delegate-active '
      if (this.delegate.isValidator) value += 'li-delegate-validator '
      return value
    },
    vpMax () {
      if (this.delegates.delegates.length > 0) {
        let richestDelegate = maxBy(this.delegates.delegates, 'voting_power')
        return richestDelegate.voting_power
      } else {
        return 0
      }
    },
    vpTotal () {
      return this.delegates.delegates
        .slice()
        .sort((a, b) => b.voting_power - a.voting_power)
        .slice(0, 100)
        .reduce((sum, v) => sum + v.voting_power, 0)
    },
    vpStyles () {
      let percentage = Math.round((this.delegate.voting_power / this.vpMax) * 100)
      return { width: percentage + '%' }
    },
    bondedPercent () {
      return this.delegate.voting_power / this.vpTotal
    },
    inCart () {
      return this.shoppingCart.find(c => c.id === this.delegate.id)
    },
    userCanDelegate () {
      return this.user.atoms > 0
    },
    delegateType () {
      return this.delegate.isValidator ? 'Validator' : 'Candidate'
    }
  },
  data: () => ({ num: num }),
  methods: {
    add (delegate) {
      this.$store.commit('addToCart', delegate)
    },
    rm (delegate) {
      this.$store.commit('removeFromCart', delegate.id)
    }
  },
  watch: {
    yourVotes (newVal, oldVal) {
      if (newVal > 0) {
        this.$store.commit('addToCart', this.delegate)
      }
    }
  }
}
</script>

<style lang="stylus">
@require '~variables'

.li-delegate
  &:nth-of-type(2n-1)
    background app-fg
  &.li-delegate-active
    background alpha(mc, 8%)
    .li-delegate__value i
      color link

.li-delegate__values
  display flex
  height 3rem

.li-delegate__value
  flex 1
  display flex
  align-items center
  min-width 0

  &:last-child
    flex 0.5

  &.name
    flex 2
    padding-left 1rem

    span a
      display flex
    .li-delegate__icon
      width 1.5rem
      display flex
      align-items center
      justify-content center
      img, span
        height 1rem
        width 1rem

  &.bar
    position relative
    span
      display block
      position absolute
      top 0
      left 0
      z-index z(listItem)

      line-height 3rem
      color txt

    .bar
      height 1.5rem
      position relative
      left -0.25rem
      background alpha(accent, 33.3%)

  &.checkbox
    justify-content center
    cursor pointer

  span
    white-space nowrap
    overflow hidden
    text-overflow ellipsis
    padding-right 1rem

.sort-by.name
  padding-left 1rem
</style>
