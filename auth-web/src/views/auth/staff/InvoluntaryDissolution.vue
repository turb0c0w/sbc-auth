<template>
  <v-container
    id="involuntary-dissolution"
    class="view-container"
  >
    <div class="view-header flex-column">
      <h1>
        Staff Involuntary Dissolution Batch
      </h1>
      <p class="mt-2 mb-0">
        B.C. Business Ready for D1 Dissolution: {{ businessesReadyforDissolutionNumber }}
      </p>
    </div>

    <!-- Automated Dissolution Section -->
    <section>
      <v-row>
        <v-col
          cols="12"
          lg="9"
        >
          <header>
            <h2>Automated Dissolution</h2>
            <p class="mt-2">
              You can set up a schedule to automate the involuntary dissolution process.
              The system will prioritize the oldest eligible businesses to move into D1 dissolution,
              automatically saving a list of businesses in each batch to the LAN.
              The schedule will run until there are no more businesses ready for D1 dissolution.
            </p>
          </header>
        </v-col>
      </v-row>
      <v-row>
        <v-col
          cols="12"
          lg="9"
        >
          <v-card
            id="company-summary-vcard"
            flat
            class="mt-2"
          >
            <CardHeader
              badgeText="Paused"
              icon="mdi-calendar-clock"
              label="Automated Dissolution Schedule"
              :showBadge="isOnHold"
            />
            <DissolutionSchedule
              @update:onHold="isOnHold=$event"
            />
          </v-card>
        </v-col>
      </v-row>
    </section>
  </v-container>
</template>

<script lang="ts">
import { computed, defineComponent, onMounted, ref } from '@vue/composition-api'
import { CardHeader } from '@/components'
import DissolutionSchedule from '@/components/auth/staff/DissolutionSchedule.vue'
import { useStaffStore } from '@/stores/staff'

export default defineComponent({
  name: 'InvoluntaryDissolution',
  components: {
    CardHeader,
    DissolutionSchedule
  },
  setup () {
    const isOnHold = ref<boolean>(false)
    const staffStore = useStaffStore()

    onMounted(() => {
      isOnHold.value = staffStore.isDissolutionBatchOnHold()
    })

    /**
     * The number of B.C. businesses that are ready for D1 Dissolution.
     * TODO: Change this once the BE is done.
     */
    const businessesReadyforDissolutionNumber = computed(() => 0)

    return {
      businessesReadyforDissolutionNumber,
      isOnHold
    }
  }
})
</script>

<style lang="scss" scoped>
@import '@/assets/scss/theme.scss';
h2 {
  font-size: $px-18;
}

p {
  font-size: $px-16;
}

// Tighten up some of the spacing between rows
[class^="col"] {
  padding-top: 0;
}
</style>
