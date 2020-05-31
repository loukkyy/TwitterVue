<template>
    <div class="tweet-container">
        <h1 class="tweet-header">@{{ tweetItem.author }} said {{ elapsedTime() }} </h1>
        <p>{{ tweetItem.message }}</p>
    </div>
</template>

<script>
export default {
  name: 'TweetItem',
  props: {
    tweetItem: Object
  },
  data () {
      return {
          now: Date.now(),
          intervals:{
            second: 1000,
            minute: 1000 * 60,
            hour: 1000 * 60 * 60,
            day: 1000 * 60 * 60 * 24
        },
        show: false
      };
  },
  methods: {
      elapsedTime() {
        let diff = Math.abs(this.now - this.tweetItem.timestamp);
        let interval = null;
        let unit = null;
        if (diff < (30 * this.intervals.second)) return "now"; // below 30 seconds
        
        if (diff < this.intervals.minute) { // below 1 minute
            interval = this.intervals.second;
            unit = "second";
        }
        else if (diff < this.intervals.hour) { // below 1 hour
            interval = this.intervals.minute;
            unit = "minute";
        }
        else if (diff < this.intervals.day) { // below 1 day
            interval = this.intervals.hour;
            unit = "hour";
        }
        else { // beyond 1 day
            interval = this.intervals.day;
            unit = "day";
        }
        let elapsedInterval = Math.trunc(diff / interval);
        return `${elapsedInterval} ${unit}${elapsedInterval > 1 ? "s" : ""} ago`;
      }
  },
  mounted() {
       this.interval = setInterval(() => {
        this.now = Date.now()
    }, 30000)
    this.show=true;
  }
}
</script>

<style scoped>
    .tweet-header {
        font-size: 1rem;
    }
    .tweet-container {
        border-radius: .5em;;
        border: 2px solid black;
        max-width: 70vw;
        overflow-wrap: break-word;
        padding: .5em 1em;
        transition: all .4s ease;
    }
</style>