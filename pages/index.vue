<template>
  <div class="container mx-auto">
    <div class="box-border p-4 border-1 border-gray-400 bg-gray-200">
      <select
        class="bg-white focus:outline-none focus:shadow-outline border border-gray-300 rounded-lg py-2 px-4 block appearance-none leading-normal inline-block w-40"
      >
        <option>GET</option>
      </select>
      <input
        v-model="url"
        class="bg-white focus:outline-none focus:shadow-outline border border-gray-300 rounded-lg py-2 px-4 block appearance-none leading-normal inline-block w-1/2"
        type="text"
        placeholder="Enter URL"
      />
      <input
        v-model="numberOfRequests"
        class="bg-white focus:outline-none focus:shadow-outline border border-gray-300 rounded-lg py-2 px-4 block appearance-none leading-normal inline-block w-1/5"
        type="number"
        placeholder="Enter Number of Requests"
      />
      <button
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded inline-block w-40"
        @click="start()"
      >Execute</button>

      <ul>
        <li
          v-for="(request,index) of requests"
          :key="index"
          class="box-content p-2 mt-2 border-2 border-gray-400 bg-gray-200"
        >{{ request.label }} - {{ request.responseTime }}</li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  data() {
    return {
      url: "https://postman-echo.com/get?foo1=bar1&foo2=bar2",
      numberOfRequests: 0,
      requests: [] as any[],
    };
  },
  methods: {
    start() {
      for (let iteration = 0; iteration < this.numberOfRequests; iteration++) {
        this.requests.push({
          label: this.url,
          responseTime: 0,
          startTimer: performance.now(),
          endTimer: "",
        });

        fetch(this.url, { mode: "no-cors" }).then((res: any) => {
          console.log(res);
          this.requests[iteration].endTimer = performance.now();
          this.requests[iteration].responseTime = Number(
            this.requests[iteration].endTimer -
              this.requests[iteration].startTimer
          ).toFixed(2);
        });
      }
    },
  },
});
</script>

<style>
</style>
