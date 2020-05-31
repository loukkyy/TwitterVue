<template>
<div class="container">
  <h1>Twitter Vue</h1>
  <div class="twitter-box">
    <form @submit.prevent="validateForm" class="form">
      <div class="text-input">
        <input type="text" id="name-input" autocomplete="off" required v-model="tweet.author" />
        <label for="name-input" class="label-name">
          <span class="label-content">Name</span>
        </label>
      </div>
      <p v-if="errors.name">Please enter your name</p>
      <div class="message-input">
        <textarea id="tweet-textarea" required v-model="tweet.message" :maxlength="maxCharacters"
        @keydown.enter.exact.prevent
        @keyup.enter.exact="validateForm"
        @keyup.enter.shift.exact="newline"
    ></textarea>
        <label for="tweet-textarea" class="label-textarea">
          Message ({{remainingCharacters}} left)
        </label>
      </div>
      <div class="btn-container">
        <button @click="validateForm" class="btn" 
        :disabled="!remainingCharacters || tweet.message.length == 0 || tweet.author.length == 0">Send</button>
      </div>
    </form>
      
  </div>
  <div class="tweet-list">
    <transition-group name="list" tag="p">
    <ul v-for="tweet in sortedTweets" :key="tweet.timestamp.toString()">
      <li>
        <TweetItem :tweetItem="tweet"/>
      </li>
    </ul>

  </transition-group>
  </div>
</div>
</template>

<script>
import TweetItem from './TweetItem'
const MAX_TWEET_LENGTH = 280;
export default {
  name: 'HelloWorld',
  components: {
    TweetItem
  },
  props: {
    msg: String
  },
  data() {
    return {
      tweets: [
        { timestamp: new Date('May 21 2020 18:25:30'), author: "Bob", message: "Can't wait to see Dragon in orbit!" },
        { timestamp: new Date('May 29 2020 22:41:30'), author: "Bob", message: "Launch has been cancelled for today!" },
        { timestamp: new Date('May 30 2020 21:40:30'), author: "Bob", message: "Successful launch of Falcon 9!" },
        { timestamp: new Date('May 30 2020 21:56:30'), author: "Joe", message: "Dragon capsule is on orbit!" },
        { timestamp: new Date('May 31 2020 12:22:30'), author: "Jane", message: "Dragon capsule is attached to ISS!" },
        { timestamp: new Date('May 31 2020 19:10:30'), author: "Jane", message: "Let's visit the universe!" }
      ],
      tweet: {
        timestamp: null,
        author: "",
        message: ""
      },
      maxCharacters: MAX_TWEET_LENGTH,
      errors: {
        name: false,
        message: false
      }
    };
  },
  methods: {
    sendTweet() {
      this.tweet.timestamp = new Date(Date.now());
      this.tweets.push({...this.tweet});
      
      this.tweet.timestamp = "";
      this.tweet.author = "";
      this.tweet.message = "";
      this.errors.name = false;
      this.errors.message = false;

      console.log(this.tweets);
    },
    validateForm() {
      let isError = false;
      if (this.tweet.author.length == 0) {
        this.errors.name = true;
        isError = true;
      } 
      if (this.tweet.message.length == 0) {
        this.errors.message = true;
        isError = true;
      } 
      if (isError == false) this.sendTweet();
    }
  },
  computed: {
    sortedTweets() {
      return this.tweets.slice().sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp))
    },
    remainingCharacters() {
      return MAX_TWEET_LENGTH - this.tweet.message.length;
    }
  }
}
</script>

<style scoped>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100vw;
  }
  .twitter-box {
    display: flex;
    justify-content: space-around;
    flex-direction: column;
    width: 80%;
    padding: 2em;
    /* background-color: rgb(152, 221, 255); */
  }
  .form {
    display: flex;
    flex-direction: column;
  }
  .tweet-list {
    display: flex;
    flex-direction: column;
  }
  .tweet-list ul {
    margin: 0;
    padding: 0;
    list-style: none;
    margin-bottom: 1rem;
  }
  .btn-container {
    text-align: end;
  }

  /* text-input */

  .text-input {
    position: relative;
    width: 100%;
    margin: 2px;
    overflow: hidden;
  }
  .text-input input {
    width: 100%;
    height: 100%;
    padding-top: 1em;
    border: none;
    outline: none;
    background-color: transparent;
    font-size: inherit;
  }
  .text-input input:focus + .label-name .label-content,
  .text-input input:valid + .label-name .label-content {
    transform: translateY(-1em);
    font-size: .8em;
    font-weight: bold;
    color: hsl(202, 88%, 53%);
  }
  .text-input input:focus + .label-name::after {
    transform: translateX(0%);
  }
  .text-input label {
    position: absolute;
    bottom: 0px;
    left: 0px;
    width: 100%;
    height: 100%;
    pointer-events: none;
    border-bottom: 2px solid black;
  }
  .text-input label::after {
    content: "";
    position: absolute;
    bottom: -2px;
    height: 100%;
    width: 100%;
    border-bottom: 2px solid hsl(202, 88%, 53%);
    transform: translateX(-100%);
    transition: transform .3s ease;
  }

  .label-content {
    position: absolute;
    bottom: 5px;
    left: 0px;
    transition: all 0.3s ease;
  }

  /* message-input */

  .message-input {
    position: relative;
    margin-top: 2em;
  }
  .message-input label {
    position: absolute;
    top: .8em;
    left: .5em;
    pointer-events: none;
    transition: all 0.3s ease;
  }

  .message-input textarea {
    height: 100%;
    width: 100%;
    padding: 1em .5em;
    outline: none;
    border: 2px solid black;
    border-radius: .5em;
    color: inherit;
    font-family: inherit;
    font-size: inherit;
    resize: none;
    background-color: transparent;
  }
  .message-input textarea:focus {
    border-color: hsl(202, 88%, 53%);
  }
  .message-input textarea:focus + label,
  .message-input textarea:valid + label {
    transform: translateY(-2.4em);
    font-size: .8em;
    font-weight: bold;
    color: hsl(202, 88%, 53%);
    left: 0;
  }

  list-item {
  display: inline-block;
  margin-right: 10px;
}
.list-enter-active, .list-leave-active {
  transition: all 1s;
}
.list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
  opacity: 0;
  transform: translateY(-50px);
}
</style>
