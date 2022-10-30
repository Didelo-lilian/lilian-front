<template>
  <section>
    <div class="cours-template">
      <h1>{{ studentInfoMin?.level }} - {{ capitalize(studentInfoMin?.name) }}</h1>
      <ul class="parent">
        <div v-for="studentLessonMonth in studentLessons?.months" :key="studentLessonMonth.month">
          <li>{{ MONTHS[studentLessonMonth.month.split('/')[0]] }} - {{ studentLessonMonth.month.split('/')[1] }} :</li>
          <ul>
            <li v-for="studentLesson in studentLessonMonth.lessons" :key="studentLesson.title"> {{ studentLesson.day }}
              - <a :href="studentLesson.link" alt="{{ studentLesson.title }}">{{ studentLesson.title }}</a></li>
          </ul>
        </div>

        <div>
          <li>Utilitaires:</li>
          <ul>
            <div v-for="(util, index3) in utils" :key="'util-' + index3">
              <li>
                <a :href="util.link">{{ util.name }}</a>
              </li>
            </div>
          </ul>
        </div>
      </ul>
    </div>
  </section>
</template>

<script lang="ts">

/*

<div class="school-template">
			<h1>{{ school?.title }}</h1>
			<ul class="parent">
				<div
					v-for="(content, index) in school?.contents"
					:key="'content-' + index"
				>
					<li>{{ content.title }}</li>
					<ul>
						<li
							v-for="(subContent, index2) in content.subcontents"
							:key="'sub-content-' + index2"
						>
							<a :href="'https://data.lilian.didelo.fr/school/' + school?.level + '/' + realTitle  + '/' + subContent?.link">{{ subContent?.title }}</a>
						</li>
					</ul>
				</div>
			</ul>
		</div>
 */
import {defineComponent, PropType} from "vue";
import _axios from '@/plugins/axios';

interface DataComponent {
  studentInfoMin: {
    name: string;
    level: string;
  } | null;
  studentLessons: {
    name: string;
    months: {
      month: number;
      lessons: {
        day: number;
        title: string;
        link: string;
      }[]
    }
  } | null;
  utils: {
    title: string;
    link: string;
  }[] | null;
  MONTHS: string[];
}

const MONTHS = ["Janvier", "Fevrier", "Mars", "Avril", "Mai", "Juin", "Juillet", "Août", "Septembre", "Octobre", "Novembre", "Décembre"]

export default defineComponent({
  name: "Students",

  props: {
    name: {
      type: String as PropType<string>,
      required: true,
      default: "kevin-j",
    },
  },

  data: (): DataComponent => ({
    studentInfoMin: null,
    studentLessons: null,
    utils: null,
    MONTHS: ["Janvier", "Fevrier", "Mars", "Avril", "Mai", "Juin", "Juillet", "Août", "Septembre", "Octobre", "Novembre", "Décembre"],
  }),

  mounted() {
    this.loadStudent();
    this.loadUtils();
  },

  watch: {
    name() {
      this.loadStudent();
    },
  },

  methods: {
    capitalize(str): string {
      if (str) {
        if (str == "") return "";
        if (str.length === 1) return str.toUpperCase();
        str = str.replaceAll("-", " ");
        return str.charAt(0).toUpperCase() + str.slice(1);
      } else {
        return "";
      }
    },
    loadStudent(): void {
      _axios
          .get('v2/student/' + this.name)
          .then((response) => {
            this.studentInfoMin = response.data;
            this.loadStudentLesson();
          })
    },
    loadStudentLesson(): void {
      _axios
          .get('v2/studentLesson')
          .then((response) => {
            this.studentLessons = response.data[0];
          })
    },
    loadUtils(): void {
      _axios
          .get(`/v2/studentLesson/utils`)
          .then((response) => {
            this.utils = response.data;
          });
    },
  },
});
</script>

<style lang="scss" scoped>
section {
  border: 1px solid white;
  overflow: none;

  .parent {
    width: 100%;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(450px, 1fr));
    grid-gap: 2vh 5vw;
    margin-bottom: 2vh;
  }

  .parent div li {
    width: 100%;
    font-size: 1.5rem;
  }

  h1 {
    text-align: center;
    font-size: 3rem;
  }

  a:hover {
    color: #ff0000;
  }
}
</style>
