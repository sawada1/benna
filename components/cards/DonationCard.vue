<template>
  <div class="flex justify-center md:w-[80%] lg:w-[60%] sm:w-[100%] h-fit  mb-4">
    <div class="w-4/5 p-8 border border-gray-300 rounded-lg shadow-sm bg-white">
      <!-- Title with Badge -->
      <Title title="تفعيل استقطاع جديد" badge="1" class="mb-6" />

      <!-- Donation Amount Selection -->
      <div class="mt-6">
        <label class="block text-dark font-bold text-sm mb-3">اختر مبلغ المتبرع</label>
        <div class="flex flex-wrap gap-3 items-center">
          <UButton v-for="amount in amounts" :key="amount" :label="amount + ' ر.س'"
            @click="donorStore.setAmount(amount)" variant="outline" color="selector"
            class="px-5 py-2.5 rounded-lg min-w-[80px]" :class="{
              'bg-[#138B96] text-white': donorStore.selectedAmount === amount,
            }" />
          <div class="flex flex-col items-end flex-1">
            <UInput v-model="donorStore.customAmount" placeholder="مبلغ آخر" class="w-full border-gray-300 px-5 py-2.5"
              color="white" varient="solid" @input="donorStore.setCustomAmount($event.target.value)" />

          </div>
        </div>

        <p v-if="donorStore.errors.amount" class="text-red-500 text-xs mt-1">
          {{ donorStore.errors.amount }}
        </p>
      </div>

      <!-- Start Date and Recurring Donation Type -->
      <div class="mt-8">
        <div class="flex justify-between items-start gap-6">
          <!-- Start Date -->
          <div class="flex-1">
            <label class="block text-dark font-bold text-sm mb-5">تاريخ بدء الاستقطاع الدوري</label>
            <!-- <div class="flex flex-col "> -->
              <!-- <UInput type="date" v-model="donorStore.startDate" class="w-full border-gray-300 px-5 py-2.5 rounded-lg "
                @change="donorStore.setStartDate(donorStore.startDate)" color="white" variant="outline" />
              <p v-if="donorStore.errors.startDate" class="text-red-500 text-xs mt-1">
                {{ donorStore.errors.startDate }}
              </p> -->


              <UPopover :popper="{ placement: 'bottom-start' }">
                <UButton icon="i-heroicons-calendar-days-20-solid" :label="format(date, 'd MMM, yyy')" class="w-full px-5 py-2.5"  color="icon"  />

                <template #panel="{ close }">
                  <DatePicker v-model="date" is-required @close="close" @click="HandleDate" />
                </template>
              </UPopover>

              <p v-if="donorStore.errors.startDate" class="text-red-500 text-xs mt-1">
                {{ donorStore.errors.startDate }}
              </p>


            <!-- </div> -->
          </div>



          <!-- Recurring Donation Type -->
          <div class="flex-1">
            <label class="block text-dark font-bold text-sm mb-5">نوع الاستقطاع الدوري</label>
            <div class="flex gap-3">
              <UButton v-for="type in types" :key="type.label" :label="type.label"
                @click="donorStore.setRecurringType(type.value)" variant="outline" color="selector"
                class="px-5 py-2.5 rounded-lg min-w-[80px]" :class="{
                  'bg-[#138B96] text-white':
                    donorStore.recurringType === type.value,
                }" />
            </div>
            <p v-if="donorStore.errors.recurringType" class="text-red-500 text-xs mt-1 text-right">
              {{ donorStore.errors.recurringType }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import Title from "@/components/ui/Title.vue";
import { useDonorStore } from "@/stores/donation/donorStore";
import { format } from 'date-fns'

const date = ref(new Date());
const donorStore = useDonorStore();
const amounts = [5, 10, 50, 100];
const types = [
  { label: "شهري", value: "monthly" },
  { label: "اسبوعي", value: "weekly" },
  { label: "يومي", value: "daily" },
];

const HandleDate = () => {
  donorStore.setStartDate(date.value);

}

</script>

<style></style>