<script>
import axios from "axios";
import { Twitter } from "lucide-vue-next";

export default {
  components: { Twitter },
  data() {
    return {
      quoteText: "",
      authorName: "",
      gradient: {
        style:
          "background: linear-gradient(-225deg, rgb(183, 248, 219) 0%, rgb(80, 167, 194) 100%); -webkit-background-clip: text;",
      },
      shareUrl: {
        twitter: `https://twitter.com/intent/tweet?text=${this.quoteText}⧵n${this.quoteAuthor}&url=erensarac.github.io`,
      },
    };
  },
  methods: {
    createQuote() {
      axios.get("https://type.fit/api/quotes").then((res) => {
        const randomNumber = Math.floor(Math.random() * res.data.length);
        this.quoteText = res.data[randomNumber].text;
        this.authorName = res.data[randomNumber].author.replace("type.fit", " ").replace(",", " ")
      });

      axios.get("../../gradient.json").then((res) => {
        const randomGradient = Math.floor(Math.random() * res.data.length);
        this.gradient.style = res.data[randomGradient];
      });
    },
  },
  mounted() {
    this.createQuote();
  },
};
</script>

<template>
  <div id="quote-box">
    <h1 id="text" v-bind="gradient">{{ quoteText }}</h1>
    <strong id="author" v-bind="gradient">{{
      authorName === null ? "Unknown" : authorName
    }}</strong>
    <div id="footer">
      <a
        target="_blank"
        :href="`https://twitter.com/intent/tweet?text=${quoteText}%0A-%20${authorName}%0A&url=erensarac.github.io/random-quote-machine`"
        id="tweet-quote"
      >
        <Twitter color="white" :size="22" />
      </a>
      <button id="new-quote" @click="createQuote()">New Quote</button>
    </div>
  </div>
</template>

<style scoped>
#quote-box {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

#text,
#author {
  -webkit-text-fill-color: transparent;
  text-transform: uppercase;
}

#text {
  font-size: 2.5rem;
  font-weight: 900;
}

#author {
  width: 100%;
  text-align: left;
  opacity: 60%;
  font-size: 2.25rem;
  font-weight: 900;
}

#footer {
  padding: 25px 0;
  display: flex;
  gap: 10px;
}

#new-quote {
  cursor: pointer;
  outline: 0;
  border: 0;
  padding: 6px 12px;
  font-size: 1rem;
  font-family: inherit;
  font-weight: 600;
  border-radius: 8px;
  color: #fff;
  background-color: #212121;
}

#tweet-quote {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  gap: 10px;
  justify-content: space-between;
  padding: 6px 12px;
  border-radius: 8px;
  color: #fff;
  background-color: #212121;
  font-size: 1rem;
  font-family: inherit;
  font-weight: 600;
  text-decoration: none;
  transition: 250ms;
}

#tweet-quote:hover,
#new-quote:hover {
  background-color: #42b883;
}

#tweet-quote:hover svg {
  fill: #fff;
}
</style>
