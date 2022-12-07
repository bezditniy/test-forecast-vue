<script>

export default {
  name: "get-request-async-await",
  data() {
    return {
      forecastList: [],
      sortedbyASC: true,
      cities: [{lat: 51.51, long: -0.13, city: "London"},{lat: 52.52, long: 13.41, city: "Berlin"}, {lat: 48.85, long: 2.35, city: "Paris"},{lat: 40.71, long: -74.01, city: "New York"}, {lat: 53.33, long: -6.25, city: "Dublin"},{lat: 50.09, long: 14.42, city: "Prague"}]
    };
  },

  async created() {
    for (const element of this.cities) {
      const response = await fetch(`https://api.open-meteo.com/v1/forecast?latitude=${element.lat}&longitude=${element.long}&current_weather=true&daily=apparent_temperature_max&daily=apparent_temperature_min&daily_units=apparent_temperature_max&timezone=auto`);
      this.forecastList.push(await response.json())
    }
  },

  methods: {
    sortList(sortBy) {
      if (this.sortedbyASC) {
        this.forecastList.sort((x, y) => (x[sortBy] > y[sortBy] ? -1 : 1));
        this.forecastList.sort((x, y) => (x.daily[sortBy] > y.daily[sortBy] ? -1 : 1));
        this.sortedbyASC = false;
        console.log(this.forecastList[0].city, true)
      } else {
        this.forecastList.sort((x, y) => (x[sortBy] < y[sortBy] ? -1 : 1));
        this.forecastList.sort((x, y) => (x.daily[sortBy] < y.daily[sortBy] ? -1 : 1));
        this.sortedbyASC = true;
        console.log(this.forecastList, false)
      }
    },
    deleteEvent: function(event) {
      this.forecastList.splice(this.forecastList.indexOf(event), 1);
      console.log(this.forecastList.splice(event, 1))
    },
  },
};
</script>

<template>
    <table>
        <tr>
          <th @click="sortList('timezone')">City</th>
          <th @click="sortList('apparent_temperature_max')">MinTemp</th>
          <th @click="sortList('apparent_temperature_max')">MaxTemp</th>
          <th @click="sortList('time')">Date</th>
        </tr>
        <tr v-for="item, index in forecastList" :key="item.timezone">
            <td>{{item.timezone.split("/")[1]}}</td>
            <td>{{item.daily.apparent_temperature_max[0]}}</td>
            <td>{{item.daily.apparent_temperature_min[0]}}</td>
            <td>{{item.daily.time[0]}}</td>
            <button @click="deleteEvent(index)"><img src="./icons/bin-delete-garbage-svgrepo-com.svg" alt="Delete"></button>
        </tr>
    </table>
</template>

<style scoped>

</style>
