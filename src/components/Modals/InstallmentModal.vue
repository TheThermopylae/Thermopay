<template>
  <div class="w-full h-full fixed top-0 right-0 z-20">
    <div class="blur w-full h-full" @click="$emit('closeModal')"></div>
    <div
      class="z-10 w-11/12 lg:w-2/5 fixed right-1/2 top-1/2 translate-x-1/2 -translate-y-1/2 bg-base-200 dark:bg-gray-800 rounded-md p-3 pt-1"
    >
      <span class="text-2xl cursor-pointer" @click="$emit('closeModal')"
        >x</span
      >
      <div class="text-2xl mt-2">
        <p>افزودن یادآوری</p>
      </div>
      <div class="my-5 grid grid-cols-1 lg:grid-cols-2 gap-5">
        <div>
          <label
            for="installment-title"
            class="text-lg after:content-['*'] after:text-red-500"
            >عنوان قسط</label
          >
          <input
            type="text"
            id="installment-title"
            class="outline-none border border-primary w-full rounded-full p-2 mt-2"
            placeholder="عنوان قسط را وارد کنید..."
            v-model="data.title"
          />
        </div>
        <div>
          <label
            for="installment-cat"
            class="text-lg after:content-['*'] after:text-red-500"
            >دسته بندی قسط</label
          >
          <select
            id="installment-cat"
            class="cursor-pointer border border-primary w-full rounded-full p-2 mt-2 outline-none bg-white dark:bg-gray-700"
            v-model="data.selectCategory"
          >
            <option value="" disabled>دسته بندی را انتخاب کنید</option>
            <option
              class="cursor-pointer"
              v-for="item in categories"
              :value="item"
            >
              {{ item }}
            </option>
          </select>
        </div>
        <div>
          <label
            for="installment-count"
            class="after:content-['*'] after:text-red-500"
            >تعداد اقساط</label
          >
          <input
            type="number"
            id="installment-count"
            class="outline-none border border-primary w-full rounded-full p-2 mt-2"
            placeholder="تعداد اقساط را وارد کنید..."
            v-model="data.count"
          />
        </div>
        <div>
          <label
            for="installment-price"
            class="after:content-['*'] after:text-red-500"
            >هزینه اقساط</label
          >
          <input
            type="number"
            id="installment-price"
            class="outline-none border border-primary w-full rounded-full p-2 mt-2"
            placeholder="هزینه اقساط را وارد کنید..."
            v-model="data.price"
          />
        </div>
      </div>
      <button
        class="btn btn-primary text-white rounded-full w-full mt-5"
        v-if="!loading"
        @click="
          checkValidate(
            data,
            `https://thermopay-174f7-default-rtdb.firebaseio.com/installments.json`,
            'POST',
            data,
            emit
          )
        "
      >
        افزودن یادآوری
      </button>
      <button
        class="btn btn-primary text-white rounded-full w-full mt-5"
        v-else
      >
        <LoadingSpinner class="w-7 h-7"></LoadingSpinner>
      </button>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'
import LoadingSpinner from '.././LoadingSpinner.vue'
import DateHook from '../../hooks/DateHook';
import InstallmentValidate from '../../hooks/InstallmentValidate';

    let categories = ['خانه', 'ماشین', 'وسایل خانه', 'وسایل الکتریکی', 'دیگر']

    let [day, month, year] = DateHook()

    let emit = defineEmits()

    let data = reactive({
      title: '',
      selectCategory: '',
      count: '',
      price: '',
      month: month,
      day: day,
      year: year
    })

    const [loading, checkValidate] = InstallmentValidate()
</script>