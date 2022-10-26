<template>
  <div class="home">
    <main>
      <div v-for="article in articles" :key="article.language">
        <article>
          <p v-for="(paragraph, index) in article.paragraphs" :key=index>
            {{ paragraph }}
          </p>
        </article>
        <hr/>
      </div>
    </main>
  </div>

</template>

<script lang="ts">
import {defineComponent} from "vue";
import {AxiosResponse} from "axios";
import _axios from "@/plugins/axios";

interface Article {
  language: string;
  paragraphs: string[];
}

interface DataComponent {
  articles: null | Article[];
  priorityLanguages: string[];
  nonPriorityLanguages: string[];
}

interface ResponseAux {
  noLanguage: number;
  paragraphs: string[];
}


export default defineComponent({
  name: "Home",
  data(): DataComponent {
    return {
      articles: [],
      priorityLanguages: ["French", "English"],
      nonPriorityLanguages: ["Spanish", "German"]
    };
  },
  methods: {
    async getArticles(languages: string[]) {
      for (const language of languages) {
        await _axios
            .get(`v2/home/${language}`)
            .then((response: AxiosResponse<ResponseAux[]>) => {
              const articles: Article[] = [];
              for (const article of response.data) {
                articles.push({
                  language: language,
                  paragraphs: article.paragraphs
                });
              }
              if (this.articles) {
                this.articles = this.articles.concat(articles);
              } else {
                this.articles = articles;
              }
            })
            .catch((error) => {
              console.log(error);
            });
      }
    }
  },
  mounted() {
    if (this.priorityLanguages) {
      this.getArticles(this.priorityLanguages);
    }
    if (this.nonPriorityLanguages) {
      this.getArticles(this.nonPriorityLanguages);
    }
  },
});
</script>

<style scoped lang="scss">
.home {
  main {
    width: 80vw;
    margin-top: 10vh;
    margin-bottom: 10vw;

    div {
      margin: 0 2.5vw;

      article p {
        font-size: 1.5rem;
        text-align: justify;
        width: 70vw;
        margin-left: 2.5vw;

        :first-child {
          font-size: 2rem;
          font-weight: bold;
          text-align: center;
        }
      }
    }
  }

  hr {
    display: block;
    border: 1px solid white;
    width: 40vw;
    text-align: center;
  }
}
</style>
